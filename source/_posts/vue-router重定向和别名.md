---
title: vue-router重定向和别名
date: 2020-03-05 08:30:07
tags:
  - VueRoute
categories:
  - VueRoute
---

## 重定向

重定向也是通过 `toutes` 配置来完成的.

```js
const router = new VueRouter({
  routes: [
    {
      path: '/a',
      redirect: '/b'
    }
  ]
})
```

重定向的目标也可以是一个命名的路由:

```js
const router = new VueRouter({
  routes: [
    {
      path: '/a',
      redirect: {
        name: 'foo'
      }
    }
  ]
})
```

甚至可以是一个方法, 动态返回重定向目标

```js
const router = new VueRouter({
  routes: [
    {
      path: '/a',
      redirect: to => {
        // 方法接受 目标路由 作为参数
        // return 重定向的 字符串路径/路径对象
      }
    }
  ]
})
```

> 注意 导航守卫 并没有应用在跳转路由上, 而仅仅应用在其目标上.

## 别名

`/a` 的别名是 `/b`, 意味着, 当用户访问 `/b` 时, URL 会保持为 `/b`, 但是路由匹配为 `/a`, 就像用户访问 `/a` 一样

```js
const router = new VueRouter({
  routes: [
    {
      path: '/a',
      component: A,
      alias: '/b'
    }
  ]
})
```

“别名”的功能让你可以自由地将 UI 结构映射到任意的 URL，而不是受限于配置的嵌套路由结构。
