# 通信方面FAQ

前景提要：此问题不定适用Browser通信方式，主要提供给通信发生异常时解决思路。

Q:  request请求创建过程发生延迟

【Scene】：

​				.Net的HttpWebRquest.GetRequestStreamAsync()执行前后偶发延迟（时间尝试2分钟左右)，客户端也未捕获通信异常。

【Env】：

​				Platform: <u> .Net</u>

​                Http Version: <u>v1.1</u>

【Analyze】：

  1）抓包：

![通信请求创建时发送延迟抓包](D:\GitHub-Self\FE-FAQ\cases\communication\images\通信请求创建时发送延迟抓包.png)

​       Description:通过抓包可以看到一次完整的通信过程：

​       1）建立三次握手

​       2）POST通信发送信息

​       3）响应HTTP 200，获取响应信息

​       4）四次挥手，关闭连接。

​      可以看到在第四部客户端和服务端都进行尝试关闭连接的过程中发生了异常，服务端返回的500错误，第二条通信建立第三次握手则就出现了延迟。

A:这是个综合性问题，根本的解决问题，需要客户端和服务端都进行优化和修改。

【Conclusion】：

​		初步分析，大概由于以下几方面问题影响：

   1. 客户端使用的是http1.1存在队头阻塞问题，则会出现下个请求等待上一个异常通信连接关闭后才能正常运行，连接关闭的需要等待的时为2MSL。

   2. 服务端由于未对异常通信进行及时关闭和销毁，保持了大量的TIME_WAIT和CLOSE_WATI状态，则导致越繁的出现通信异常。

 【Solution】：

#### 客户端：

​		1）取消使用using(request.GetRequestStreamAsync())，使用         

```
try{
	HttpWebRequest request = WebRequest.Create(url) as HttpWebRequest;
    request.GetRequestStreamAsync();
    request.dispose();  
}catch(ex){
	//再次对request队形进行终止,预测会关闭异常的连接。
	//此种方式因未有环境实际测试，实在抱歉，望有尝试此方案的人给予答复是否可行
   request.abort(); 
   request=null;
}
```

​        因为using(request.GetRequestStreamAsync())方式等效try{}catch(error){}finally{request.dispose()}，若request.disponse()发生异常则无法捕捉错误，这也就是抓包中看到的四次挥手发生异常但未有异常日志记录的原因。

​	__其他提供性能方案:__

​	a. 使用HttpClient 和指定HTTP 2.0，因为HTTP 2.0可以多路复用可以减少此种方式

​	b. 设置ServicePointManager.DefaultConnectionLimit = 1024； //此数据并不是最优数值，可根据各自环境进行设置。

​	c. 关闭proxy代理：https://www.cnblogs.com/TianFang/archive/2011/09/18/2180741.html，可优化首次简历request连接的时间。

#### 服务端：

  

​		1）处理TIME_WAIT和CLOSE_WAIT状态：参见：https://www.cnblogs.com/felixzh/p/8351366.html

​		2）若服务端接收到异常通信时，则需要发送connection:close;来进行主动关闭异常连接

​		3）设置服务端MSL时间