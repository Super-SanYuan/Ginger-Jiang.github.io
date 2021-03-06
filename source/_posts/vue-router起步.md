---
title: vue-router起步
date: 2020-03-03 20:51:15
tags:
  - VueRoute
categories:
  - VueRoute
---

## 起步

用 Vue.js + Vue Router 创建单页面应用, 通过 Vue 组合组件来组成应用程序, 将 Vue Router 添加进来. 将组件映射到路由, 然后告诉 Vue Router 在哪里渲染他它们

```html
<script src="https://unpkg.com/vue/dist/vue.js"></script>
<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>

<div id="app">
  <h1>Hi Ginger</h1>
  <p>
    <!-- 使用 router-link 组件来导航 -->
    <!-- 通过传入 to 属性指定链接 -->
    <!-- router-link 默认会被渲染成一个 a 标签 -->
    <router-link to="/foo">Go to Foo</router-link>
    <router-link to="/bar">Go to Bar</router-link>
  </p>
  <!-- 路由出口 -->
  <!-- 路由匹配到组件将渲染在这里 -->
</div>
```

```js
// 0. 如果使用模块化机制编程，导入Vue和VueRouter，要调用 Vue.use(VueRouter)
// 1. 定义 (路由) 组件。
// 可以从其他文件 import 进来
const Foo = { template: '<div>Foo</div>' }
const Bar = { template: '<div>Bar</div>' }

// 2. 定义路由
// 每个路由应该映射一个组件。 其中"component" 可以是
// 通过 Vue.extend() 创建的组件构造器，
// 或者，只是一个组件配置对象。
const routes = [
  {
    path: '/foo',
    component: Foo
  },
  {
    path: '/bar',
    component: Bar
  }
]

// 3. 创建 router 实例, 然后传 routes 配置
const router = new VueRouter({
  routes
})

// 4. 创建和挂载根实例
const app = new Vue({
  router
}).$mount('#app')
```

通过注入路由器, 我们可以在任何组件内通过 `this.$router` 访问路由器, 也可以通过 `this.$route` 访问当前路由

```js
// Home.vue
export default {
  computed: {
    username() {
      return this.$route.params.username
    }
  },
  methods: {
    goBack() {
      window.history.length > 1 ? this.router.go(-1) : this.$router.push('/')
    }
  }
}
```

> 当 `<router-link>` 对应的路由匹配成功，将自动设置 class 属性值 .router-link-active。
