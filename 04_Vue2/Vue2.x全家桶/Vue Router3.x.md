# Vue Router

## 1.安装和配置

### 1.1 安装路由 

```shell
npm install vue-router@3  #安装vue2版本
npm install vue-router@4  #安装vue3版本
```

### 1.2 配置路由

在 `src` 目录下创建一个 `router` 文件夹，并在其中创建一个 `index.js` 文件来配置路由。

```js
// src/router/index.js
import Vue from 'vue';
import Router from 'vue-router';
import Home from '../views/Home.vue';
import About from '../views/About.vue';
 
Vue.use(Router);
 
const routes = [
  {
    path: '/',
    redirect: '/index', 
    name: 'Home',
    component: Home
  },
  {
    path: '/about',
    name: 'About',
    component: About
  }
];

// 防止连续点击多次路由报错
let routerPush = Router.prototype.push;
let routerReplace = Router.prototype.replace;
// push
Router.prototype.push = function push(location) {
  return routerPush.call(this, location).catch(err => err)
}
// replace
Router.prototype.replace = function push(location) {
  return routerReplace.call(this, location).catch(err => err)
}
// 创建 router 实例, 并传 `routes` 配置
const router = new Router({
  mode: 'history', // 使用 HTML5 History 模式
  routes
});
 
export default router;
```

### 1.3 主文件中使用路由

在 `src/main.js` 中引入并使用配置好的路由。

```js
import Vue from 'vue';
import App from './App.vue';
import router from './router';
 
Vue.config.productionTip = false;
 
new Vue({
  router,
  render: h => h(App)
}).$mount('#app');
```

## 2.路由基本使用

### 2.1 路由视图`<router-view>`

`<router-view>` 是 Vue Router 提供的组件，用于渲染匹配到的路由组件。通常放在 `App.vue` 中。

```vue
<template>
  <div id="app">
    <router-view></router-view>
  </div>
</template>
```

### 2.2 路由链接 `<router-link>`

用于创建导航链接，使用户可以点击链接以切换路由并导航到其他页面。

```vue
<router-link to="/about">About</router-link>
```

## 3.路由传参

### 3.1 声明式导航

通过<router-link>传参

#### 3.1.1 查询参数传参

```vue
<router-link to="/path?参数名=值&参数名2=值"></router-link>  
```

接收参数： `$router.query.参数名`，可以传多个参数

#### 3.1.2 动态路由传参

```js
const router = new VueRouter({
  routes: [
    // 动态路径参数 以冒号开头 这里id是参数名
    { path: '/user/:id', component: User } 
    { path: '/search/:word?', component: User }  //如果不传参数，也希望被匹配，加入?
  ]
})
```

```vue
<router-link to="/path/参数值"></router-link>  
```

接收参数：`$router.params.参数名`  例子：`$router.params.id` ，一次只能传一个参数

### 3.2 编程式导航

通过JS代码导航传参

#### 3.2.1 查询参数传参（query传参）

```js
// 1、path路径
this.$router.push('/路径?参数名1=参数值1&参数2=参数值2')  //简单写法
//完整写法
this.$router.push({
  path: '/路径',
  query: {
    参数名1: '参数值1',
    参数名2: '参数值2'
  }
})
// 2、name命名
this.$router.push({
  name: '路由名',
  query: {
    参数名1: '参数值1',
    参数名2: '参数值2'
  }
})
```

接收参数：`$route.query.参数名`

#### 3.2.2 动态路由传参

```js
// 1、path路径
this.$router.push('/路径/参数值') //简单写法
//完整写法
this.$router.push({
  path: '/路径/参数值'
})
// 2、name命名
this.$router.push({
  name: '路由名',
  params: {
    参数名: '参数值',
  }
})
```

接受参数：`$route.params.参数值`





