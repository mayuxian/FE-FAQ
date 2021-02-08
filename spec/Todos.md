TODO: 高优先级
1. 组件库需要通过link方式引入，并且theme需要压缩。
2. public中图片等资源使用nginx配置图片服务器
3. axios、vue、vuex等使用外联或index.html的script
4. 使用lodash-es而不是lodash，lodash可以支持摇树优化。参考：https://zhuanlan.zhihu.com/p/36280323
5. 使用history模式，并解决F5刷新问题。
6. login登陆页面可以使用H5开发，无需使用vue（页面无需加载vue相关功能），
7. 是否使用import 'normalize.css'; 
8. 引入：babel-polyfill   参考：https://www.cnblogs.com/Jeely/p/11231530.html  优先级高
9. 图标样式库 Font Awesome  
10. 页面懒加载实现方法 require 或import  https://blog.csdn.net/Wbiokr/article/details/78896921
    全部使用异步记载页面,页面中异步加载其他页面,则使用b页面的推荐方式,但需要引入:@babel/plugin-syntax-dynamic-import，地址:https://babeljs.io/docs/en/babel-plugin-syntax-dynamic-import/#installation
            //a页面
        import historyTab from '../../component/historyTab/historyTab.vue';
        export default {
            components: {
                historyTab
            },
        }

        //b页面
        export default {
            components: {
                historyTab: resolve => {require(['../../component/historyTab/historyTab.vue'], resolve)},//懒加载
            },
        }
        // 推荐:const page = ()=> import('./../Test.vue);
11. 通信使用vue-axios或fetch,而不使用vue-resource,原因:https://juejin.im/post/5c13cefe518825438f6b9146,使用fetch原因:因为fetch是原生js,不是XHR. https://zhuanlan.zhihu.com/p/58062212
12. 需要具备filters目录
13. 点击事件使用 @click.prevent  为什么使用?
14. 若使用async/await,则需要babel-polyfill垫片,建议换成vue-cli 4.0
15. 每个请求都要加上token https://www.jianshu.com/p/0de32a777616
16. 全局捕获异常功能
17. 多过滤可配置组件的存储有待存储到数据库,目前是window.localstorage
18. 把布局更换成 el-aside和el-main布局  [+]
19. 需要添加Array的扩展方法remove

TODO：优先级低
1. 部分获取全局引入Element-ui 参考：https://juejin.im/post/5d78ae9c6fb9a06ae7643204
2. 使用History模式，但要处理更新（优先级低）
3. var FriendlyErrorsPlugin = require('friendly-errors-webpack-plugin')  包的作用。
4. vue里面的内联样式会被打包成css文件吗？
5. 使用FileReader读取文件  https://www.jianshu.com/p/5fd16155901a  https://juejin.im/post/5a9e23d451882577b45e824e
6. 考虑是否使用graphql，nodejs服务可以使用这个。
7. 错误信息，根据返回错误内容的长度，来决定使用展现显示（警告提示框、错误提示框、组件旁错误展示）。
8. 实现webpack的proxyServer.js   备注：devServer.setup  此选项__已废弃_，并将在 v3.0.0 中被删除。应当使用 devServer.before。
9. 实现移动端,可以实现手机Siri访问网页形式.  //有待考虑,因为flutter直接可以for web、android、ios了。
10. 考虑login页面是否要和home页面进行分离为独立的html,优先级.e3
11. table表格的属性也可以动态绑定  优先级低
12. 可配置多过滤组件的字段进行可以排序.   优先级低
13. 开发规范文档  优先级低  (表格,详情,过滤的组件的使用方式)
14. Restful更新使用patch,增加使用put
15. import XLSX from 'xlsx';  Vue.use(XLSX)如何实现异步,使用时才被网页请求.

## TODO:
1. 需要添加和下载字体：ingFangSC-Regular,PingFang SC 

2. 查询条件的重置操作未统一，部分模块重置会重新查询信息列表，部分模块只是清空筛选项。建议清空筛选项，不做数据查询操作。

3. vuex过度使用，vuex只有在不同组件共享状态时才需要使用。  

4. 未实现通过token的登陆

5. 登陆页面的验证码未实现动态

6. 使用History模式，但要处理更新（优先级低）

7. 错误信息的展示处理。

   错误追踪服务:Sentry for vue - Raven.js   https://www.jianshu.com/p/b561c6cd308a  

8. ?
