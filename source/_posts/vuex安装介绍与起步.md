---
title: vuex安装介绍与起步
date: 2020-03-17 15:02:31
tags:
  - Vue
categories:
  - Vue
---

## 安装

- 直接下载 || CDN 引用 [vuex](https://unpkg.com/vuex)

```html
<script src="/path/to/vue.js"></script>
<script src="/path/to/vuex.js"></script>
```

- NPM

```sh
npm install vuex --save
```

- Yarn

```sh
yarn add vuex
```

- 在一个模块化的打包系统中, 必须显示地通过 `Vue.use()` 来安装 `Vuex`

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)
```

## Vuex 是什么?

Vuex 是一个专门为 Vue.js 应用程序开发的状态管理模式. 它采用集中式存储管理应用的所有组件的状态, 并已相应的规则保证状态以一种可预测的方式发生变化. Vuex 也集成到 Vue 的官方调试工具 devtools extension, 提供了诸如零配置的 time-travel 调试、状态快照导入导出等高级调试功能

## 什么是状态管理模式?

已一个简单的 Vue 记数应用开始:

```js
new Vue({
  // state
  data() {
    return {
      count: 0,
    }
  },
  // view
  template: '<div> {{ count }} </div>',
  // actions
  methods: {
    increment() {
      this.count++
    },
  },
})
```

这个状态管理应用包含以下几个部分:

- state: 驱动应用的数据源
- view: 以声明方式将 state 映射到试图
- actions: 响应在 view 上的用户输入导致的状态变化

以下是一个表示单项数据流理念的简单示意图:

![flow](../images/vue/flow.png)

当我们遇到多个组件共享状态时, 单项数据流的简洁性很容易被破坏:

- 多个视图依赖于同一状态
- 来自不同视图的行为需要变更同一状态

对于问题一, 传参的方法对于多层嵌套的组件将会非常繁琐, 并且对于兄弟组件间的状态传递无能为力
对于问题二, 我们经常会采用父子组件直接引用或者通过事件来变更和同步状态的多份拷贝.

因此, 我们为什么不把组件的共享状态抽取出来, 以一个全局单例模式管理呢? 在这种模式下, 我们组件树构成了一个巨大的试图, 不管树在那个位置, 任何组件都能获取状态或者触发行为.

通过定义和隔离状态管理中的各种概念并通过强制规则维持试图和状态间的独立性,我们的代码将会变得更结构化且易维护

这就是 Vuex 背后的基本思想，借鉴了 Flux、Redux 和 The Elm Architecture。与其他模式不同的是，Vuex 是专门为 Vue.js 设计的状态管理库，以利用 Vue.js 的细粒度数据响应机制来进行高效的状态更新

![vuex](../images/vue/vuex.png)

## 什么情况下应该使用 Vuex?

Vuex 可以帮助我们管理共享状态，并附带了更多的概念和框架。这需要对短期和长期效益进行权衡。

如果您不打算开发大型单页应用，使用 Vuex 可能是繁琐冗余的。确实是如此——如果您的应用够简单，您最好不要使用 Vuex。一个简单的 store 模式就足够您所需了。但是，如果您需要构建一个中大型单页应用，您很可能会考虑如何更好地在组件外部管理状态，Vuex 将会成为自然而然的选择。

## 开始

每个 Vuex 应用的核心就是 store. store 基本上就是个容器, 它包含着你的应用中大部分的状态(state). Vuex 和单纯的全局对象有以下两个点不同

1. Vuex 的状态存储是响应式的. 当 Vue 组件从 store 中读取状态的时候, 若 store 中的状态发生改变, 那么相应的组件也会响应的得到高效更新

2. 不能直接修改 store 中的状态. 改变 store 中的状态的唯一途径就是显示的提交(commit)mutation. 这样使得我们可以方便的跟踪没一个状态的变化, 从而让我们能够实现一些工具帮助我们更好的了解我们的应用

## 使用

安装 Vuex 之后, 我们可以创建一个 store.

```js
const store = new Vuex.Store({
  state: {
    count: 0,
  },
  mutations: {
    increment(state) {
      state.count++
    },
  },
})
```

现在, 可以通过 `store.state` 来获取状态对象, 以及通过 `store.commit` 方法触发状态变更

```js
store.commit('increment')
console.log(store.state.count) // -> 1
```

我们通过提交 mutation 的方式，而非直接改变 store.state.count，是因为我们想要更明确地追踪到状态的变化。这个简单的约定能够让你的意图更加明显，这样你在阅读代码的时候能更容易地解读应用内部的状态改变。此外，这样也让我们有机会去实现一些能记录每次状态改变，保存状态快照的调试工具。有了它，我们甚至可以实现如时间穿梭般的调试体验。

由于 store 中的状态是响应式的, 在组件中调用 store 中的状态简单到仅需要在计算属性中返回即可. 触发变化也仅仅是在组件的 methods 中提交 mutation
