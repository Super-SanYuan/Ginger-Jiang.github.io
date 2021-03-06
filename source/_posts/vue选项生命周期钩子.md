---
title: vue选项生命周期钩子
date: 2020-02-22 18:46:44
tags:
  - Vue
categories:
  - Vue
---

> 所有的生命周期钩子自动绑定 this 上下文到实例中，因此你可以访问数据，对属性和方法进行运算。这意味着你不能使用箭头函数来定义一个生命周期方法 (例如 created: () => this.fetchTodos())。这是因为箭头函数绑定了父上下文，因此 this 与你期待的 Vue 实例不同，this.fetchTodos 的行为未定义。

## beforeCreate

- 类型 Function
- 详细
  在实例初始化之后, 数据观测(data observer) 和 event/watcher 事件配置之前被调用

## created

- 类型 Function
- 详细
  在实例创建完成后被立即调用吗, 在这一步, 实例已完成以下配置: 数据观测(data observer), 属性和方法的运算, watch/event 事件回调, 然而, 挂载阶段还没开始 `$el`属性尚不可用

## beforeMount

- 类型 Function
- 详细
  在挂载开始之前被调用: 相关 `render` 函数首次被调用
  **该钩子在服务器端渲染期间不被调用**

## mounted

- 类型 Function
- 详细:
  实例被挂载后调用, 这时 `el` 被创建的 `vm.$el` 替换了, 如果根实例挂载到一个文档内的元素上, 当 `mounted` 被调用时, `vm.$el` 也在文档内的元素上, 当 `mounted` 被调用时 `vm.$el` 也在文档内
  mounted 不会保证所有的子组件也都一起被挂载, 如果你希望等到整个试图都苏安然完毕, 可以在 mounted 内部使用 `vm.$nextTick`
  **该钩子在服务器端渲染期间不被调用**
  ```js
  mounted: function() {
    this.$nextTick(function() {
      // Code that will run only after the
      // entire view has been rendred
    })
  }
  ```

## beforeUpdate

- 类型: Function
- 详细
  数据更新时调用, 发生在虚拟 DOM 打补丁之前, 这里适合在更新之前访问现有的 DOM. 比如手动移除已添加的事件监听器

## updated

- 类型 Function
- 详细
  由于数据更改导致的虚拟 DOM 重新渲染和打补丁, 在这之后会调用该钩子
  当这个钩子被调用时. 组件 DOM 已经更新. 所以现在可以执行依赖 DOM 的操作. 然而在大多数情况下，你应该避免在此期间更改状态。如果要相应状态改变，通常最好使用计算属性或 watcher 取而代之。
  updated 不会保证所有的子组件也都一起被重绘。如果你希望等到整个视图都重绘完毕，可以在 updated 里使用 vm.\$nextTick：

  ```js

  updated: function() {
    this.$nextTick(function() {
    // Code that will run only after the
    // entire view has been re-rendered
    })
  }
  ```

## activated

- 类型 Function
- 详细
  被 keep-alive 缓存的组件激活时调用
  该钩子在服务器端渲染期间不被调用

## deactivated

- 类型 Function
- 详细
  被 keep-alive 缓存的组件停用时调用

## beforeDestroy

- 类型 Function
- 详细
  实例销毁之前调用, 在这一步, 实例仍然完全可用

## destroyed

- 类型 Function
- 详细
  实例销毁后调用, 该钩子被调用后, 对应 Vue 实例的所有指令都被解绑, 所有事件监听器被移除, 所有子实例也都被销毁

## errorCaptured

- 类型 (err: Erroe, vm: Component, info: string) => ?boolean
- 详细
  当捕获一个来自子孙组件的错误时被调用。此钩子会收到三个参数：错误对象、发生错误的组件实例以及一个包含错误来源信息的字符串。此钩子可以返回 false 以阻止该错误继续向上传播。
- 错误传播规则

  - 默认情况下，如果全局的 config.errorHandler 被定义，所有的错误仍会发送它，因此这些错误仍然会向单一的分析服务的地方进行汇报。

  - 如果一个组件的继承或父级从属链路中存在多个 errorCaptured 钩子，则它们将会被相同的错误逐个唤起。

  - 如果此 errorCaptured 钩子自身抛出了一个错误，则这个新错误和原本被捕获的错误都会发送给全局的 config.errorHandler。

  - 一个 errorCaptured 钩子能够返回 false 以阻止错误继续向上传播。本质上是说“这个错误已经被搞定了且应该被忽略”。它会阻止其它任何会被这个错误唤起的 errorCaptured 钩子和全局的 config.errorHandler。
