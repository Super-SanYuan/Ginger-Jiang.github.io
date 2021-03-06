---
title: vue实例方法生命周期
date: 2020-02-26 16:16:58
tags:
  - Vue
categories:
  - Vue
---

## vm.\$mount([elementOrSeletor])

- 参数:
  - {Element | string} [elementOrSelector]
  - {boolean} [hydrating]
- 返回值: `vm` - 实例自身
- 用法:
  如果 Vue 实例在实例化时没有收到 el 选项, 则它处于未挂载状态, 没有关联的 DOM 元素.可以使用 `vm.$mount()` 手动挂载一个未挂载的实例
  如果没有提供 `elementOrSelector` 参数, 模板将被渲染为文档之外的元素, 并且你必须使用原生 DOM API 把它插入文档中
  这个方法返回实例自身, 因而可以链式调用其他实例方法
- 示例:

  ```js
  var MyComponent = Vue.extend({
    template: '<div>Hi</div>'
  })

  // 创建并挂载到 #app (会替换 #app)
  new MyComponent().$mount('#app')

  // 同上
  new MyComponent({ el: '#app' })

  // 或者，在文档之外渲染并且随后挂载
  var component = new MyComponent().$mount()
  document.getElementById('app').appendChild(component.$el)
  ```

## vm.\$forceUpdate()

- 示例
  迫使 Vue 实例重新渲染, 注意他仅仅影响实例本身和插入插槽的子组件

## vm.\$nextTick([callback])

- 参数:
  - {Function} [callback]
- 用法:
  将回调延迟到下次 DOM 更新循环之后执行. 在修改数据之后立即使用它, 然后等待 DOM 更新. 它跟全局方法 `Vue.nextTick` 一样, 不同的是回调的 `this` 自动绑定到调用它的实例上
- 示例:
  ```js
  new Vue({
    // ...
    methods: {
      // ...
      example: function() {
        // 修改数据
        this.message = 'changed'
        // DOM 现在还没更新
        this.$nextTick(function() {
          // DOM 现在更新了
          // this 绑定到当前实例
          this.doSomethingElse()
        })
      }
    }
  })
  ```

## vm.\$destroy()

- 用法:
  完全销毁一个实例. 清理他与其他实例的链接. 解绑他的全部指令及事件监听器
  触发 `beforeDestroy` 和 `destroyed` 的钩子
