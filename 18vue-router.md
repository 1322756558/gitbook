## Vue-router

- vue-router 的基本使用

1. yarn 或 npm 安装vue-router
2. 在main.js中引入及实例化

```js
import Vue from 'vue'
import App from './App.vue'
// 引入
import VueRouter from 'vue-router'
// 路由复杂 将其写入外部文件
import routes from './routes'

Vue.config.productionTip = false

// use
Vue.use(VueRouter)

const router = new VueRouter({
  // 为history时无#
  // mode: 'history',
  routes
})

new Vue({
  // 注册
  router,
  render: h => h(App),
}).$mount('#app')
```



3. 配置路由文件

```js
// 引入页面
import parentDemo from "./components/parentDemo";
import childDemo from "./components/childDemo";

const routes = [
  // 配置路由基础写法
  { path: "/foo", component: parentDemo, name: "1" },
  { path: "/bar", component: childDemo, name: "2" },
  // 带子页面的写法
  {
    // 将id传入页面
    path: '/user/:id',
    component: parentDemo,
    name: '3',
    props: true,
    children: [
      // 子页面，会显示在父页面’/user/:id‘的<fouter-view/> 中
      {
        path: 'profile',
        component: childDemo,
        name: '3-1'
      },
    ]
  },
  // 页面重定向
  { path: "/a", redirect: "/bar" },
  // 默认匹配404
  { path: "*", component: parentDemo, name: "404" },
];

export default routes
```

4. 页面引用

```html
App.vue
在主页面中引用， 后续的界面添加到router-view中
<template>
  <div id="app">
    <h2>router demo</h2>
    <router-view></router-view>
  </div>
</template>

parentDemo.vue
<template>
  <div id="parentDemo">
    this is parentDemo
    
    // 跳转方式
    <router-link to="/bar">跳转childDemo</router-link>
    <a href="#/bar">跳转childDemo</a>
    <button @click="$router.push('foo')">go to foo</button>
    // 子路由放入这里
    <router-view></router-view>
    <p>id:{{id}}</p>
  </div>
</template>

<script>
export default {
  name: 'parentDemo',
  // 通过url获取id
  props: ['id'],
  component:{

  }
}
</script>
```

