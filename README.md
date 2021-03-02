## Vue商城项目

###  项目的基本的运行步骤

```javascript
//项目安装
npm install
//项目运行
npm run serve
//项目打包
npm run build
```

### 项目中存在的知识点

1.  ##### 项目中使用的样式组件库为vant

##### 2.本项目的项目中存在很多的模块化的思想，比如头部导航栏的封装、底部导航栏的封装，轮播图的封装等；

   ![image-20210301160101860](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210301160101860.png)

   ![image-20210301154936873](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210301154936873.png)

3. ##### 本项目的网络请求

   本项目使用的是axios搭建的网络请求：

   ```javascript
   // 项目的请求地址为个人购买的地址
   const url = "http://152.136.185.210:8000/api/w6";
   
   let config = {
     baseURL: url,
     timeout: 10000
   };
   
   ```

	// 创建axios的实例
   ```javascript
const _axios = axios.create(config);

   // 请求拦截
   _axios.interceptors.request.use(
     req => {
       // 当getters里面的isLoading为true再显示请求加载
       if (Loading.getters.isLoading) {
         Toast.loading({
           forbidClick: true,
           message: "加载中..."
         });
       }
       return req;
     },
     err => {
       return Promise.reject(err);
     }
   );

   // 响应拦截
   _axios.interceptors.response.use(
     res => {
       Toast.clear();
       return res.data;
     },
     err => {
       Toast.clear();
       return Promise.reject(err);
     }
   );

   // 全局注册axios
   window.axios = _axios;
   ```

4. ##### keep-alive让组件保持当前的状态

   该项目用这个知识点的地方是：当页面之间来回切换的时候（动态组件），想要保存想一个页面的状态的时候，就需要用到该方法

   ###### 用法：

   ```javascript
   <keep-alive> 包裹动态组件时，会缓存不活动的组件实例，而不是销毁它们。和 <transition> 相似，<keep-alive> 是一个抽象组件：它自身不会渲染一个 DOM 元素，也不会出现在组件的父组件链中。
   
   当组件在 <keep-alive> 内被切换，它的 activated 和 deactivated 这两个生命周期钩子函数将会被对应执行。
   
   其中存在两个属性：include 和 exclude
   ```

   

5. ##### vuex的使用

   vuex状态管理，是该项目数据管理的仓库；

   管理数据状态的目录：

   ![image-20210301165434867](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210301165434867.png)

   + index.js   store的入口的文件：

   ​	该文件主要用于state进行数据的存储；

   + getters

     该文件主要用于对state里面的数据进行进一步的处理；

     mapGetters:对数据的处理的引入，相当于计算属性；

     ![image-20210301204618621](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210301204618621.png)

     ![image-20210301204658365](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210301204658365.png)

   + mutations

     更改vuex的store中的状态的唯一方法就是提交给mutation；

     通过**store.commit** ()的触发方式来触发相应的事件；

     mapMutations: 相当于一个方法，当存在多个mutation需要引入的时候使用该方法

     ![image-20210301204214145](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210301204214145.png)

   + Action

     类似于mutation，不同在于Action提交的是mutation，而不是直接更改状态、Action可以包含任意异步操作

   （具体请参考Vuex的官方文档）

6. ##### 首页中可滚动区域的问题

   + Better-Scroll在决定有多少区域可以滚动时, 是根据scrollerHeight属性决定
     + scrollerHeight属性是根据放Better-Scroll的content中的子组件的高度
     + 但是我们的首页中, 刚开始在计算scrollerHeight属性时, 是没有将图片计算在内的
     + 所以, 计算出来的告诉是错误的(1300+)
     + 后来图片加载进来之后有了新的高度, 但是scrollerHeight属性并没有进行更新.
     + 所以滚动出现了问题
   + 如何解决这个问题了?
     * 监听每一张图片是否加载完成, 只要有一张图片加载完成了, 执行一次refresh()
     * 如何监听图片加载完成了?
       * 原生的js监听图片: img.onload = function() {}
       * Vue中监听: @load='方法'
     * 调用scroll的refresh()

   + 如何将GoodsListItem.vue中的事件传入到Home.vue中
     + 因为涉及到非父子组件的通信, 所以这里我们选择了**事件总线**
       * bus ->总线
       * Vue.prototype.$bus = new Vue()
       * this.bus.emit('事件名称', 参数)
       * this.bus.on('事件名称', 回调函数(参数))

   + 对于refresh非常频繁的问题, 进行防抖操作

     * 防抖debounce/节流throttle(课下研究一下)
     * 防抖函数起作用的过程:
       * 如果我们直接执行refresh, 那么refresh函数会被执行30次.
       * 可以将refresh函数传入到debounce函数中, 生成一个新的函数.
       * 之后在调用非常频繁的时候, 就使用新生成的函数.
       * 而新生成的函数, 并不会非常频繁的调用, 如果下一次执行来的非常快, 那么会将上一次取消掉

     ```javascript
     debounce(func, delay) {
             let timer = null
             return function (...args) {
               if (timer) clearTimeout(timer)
               timer = setTimeout(() => {
                 func.apply(this, args)
               }, delay)
             }
           },
     ```

     

7. 

