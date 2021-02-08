## 规范

### 分支规范
参照gitlab flow的分支规范,参考地址:https://www.cnblogs.com/wish123/p/9785101.html
Master 主分支,应有发布生产环境
Release 预发布分支
Develop  测试分支,应用于测试环境
Feature_xxx  新功能特性分支
Hotfix   问题修复分支


### 目录定义规范
1. static 和 assets目录区别: static目录不会被webpack处理,assets目录资源通过相对资源引用,并会被webpack解析处理.
    static目录存放一些如商品图片等频繁修改的,assets主要存放一些背景图片等资源文件.
2. 

### 文件名定义规范
1. 交易文件夹使用短横线命名法(kebab-case)
2. 交易文件vue名称使用大骆峰式命名法（Camel-Case）


### 引用定义规范
1. import { 强名称 } from 'xx';
   原因:约定规范,便于查找和阅读等.
2. import lodash-es等库时,需要增量引入.
    原因:为了便于摇树优化,减少打包体积.
3. 某个模块的通信模块Api在各自的modules下定义,并使用强限定方式export {}方式导出,import {}方式导入.
   通信模块:[module]/Api
4. 全部使用异步记载页面,参考 https://blog.csdn.net/Wbiokr/article/details/78896921

5. 定义的jssdk导出对象使用首字母小写命名, 导出类使用首字母大写命名

### 开发技术规范

1. 静态数据、枚举数据等使用Computed,而不是放在data中提高性能.
   因为data中vue会深度遍历并增加Observer.
2. 只有需要共享或会话级别数据才需要使用Vuex

3. @click="onSave" 事件中要加on前缀

4. img 绑定图片:src使用此种方式:https://www.cnblogs.com/wzcsqaws/p/11283228.html

5. 确定按钮在右,取消按钮在左. 

6. 枚举类型的值若为int,则都从1开始.或者设置字符串'',减少判断绑定为0时表示否则的处理

7. 使用query路由跳转,params会涉及页面刷新未重新请求问题.

8. 检验规则也要让数据库提供,比如添加信息的名称的最大长度

9. 遮罩再表格上进行遮罩,编辑和新增遮罩页面. 遮罩只遮罩部分.不建议遮罩全部.

### 提醒规范
1. 限制条件,必须提示明确的正向信息,比如,金额必须大于0,而不是金额不能小于0.
