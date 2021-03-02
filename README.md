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

   

5. vuex的使用

   vuex状态管理，是该项目数据管理的仓库；

   管理数据状态的目录：

   ![image-20210301165434867](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210301165434867.png)

   + index.js   store的入口的文件：

   ​	该文件主要用于state进行数据的存储；

   + getters

     该文件主要用于对state里面的数据进行进一步的处理

   + mutations

     更改vuex的store中的状态的唯一方法就是提交给mutation；

     通过**store.commit** ()的触发方式来触发相应的事件；

   + Action

     类似于mutation，不同在于Action提交的是mutation，而不是直接更改状态、Action可以包含任意异步操作

   

   

6. 

