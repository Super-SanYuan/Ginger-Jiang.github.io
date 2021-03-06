---
title: vue选项数据
date: 2020-02-20 14:08:22
tags:
  - Vue
categories:
  - Vue
---

## data

- 类型 Object | Function
- 限制 组件的定义只接受 Function
- 详细
  Vue 实例的数据对象, Vue 将会递归将 data 的属性转换为 getter/setter, 从而让 data 的属性能够相应数据变化. 对象必须是纯粹的对象,浏览器 API 创建的原生对象, 原型上的属性会被忽略
  一旦观察过, 就无法在跟数据对象上添加响应式属性, 因此推荐在创建实例之前, 就声明所有的根级响应式属性
  实例创建之后可以通过 vm.\$data 访问原始数据对象. Vue 实例也代理了 data 对象上的所有属性, 因此访问 vm.a 等价于访问 `vm.$data.a`
  以 `_` 或 `$` 开头的属性不会被 Vue 实例代理, 因为他们可能和 Vue 内置的属性、API 方法冲突; 可以用 `vm.\$data._property` 的方式访问这些属性
  当一个组件被定义, data 必须声明为返回一个初始数据对象的函数, 因为组件可能被用来创建多个实例. 如果 data 仍然是一个纯粹的对象, 则所有的实例将 共享引用同一个数据对象. 通过提供 data 函数, 每次创建一个新实例后, 我们能调用 data 函数, 从而返回初始数据的一个全新副本数据对象
  如果需要, 可以通过将 `vm.$data` 传入 `JSOM.parse(JSON.stringify(...))` 得到深拷贝的原始数据对象
- 示例

  ```js
  var data = { a: 1 }

  // 创建一个实例
  var vm = new Vue({
    data: data
  })
  vm.a // => 1
  vm.$data === data // => true

  // Vue.extend() 中 data 必须是函数
  var Component = Vue.extend({
    data: function() {
      return { a: 1 }
    }
  })
  ```

  > 如果为 data 使用了箭头函数, 则 `this` 不会指向这个组件的实例, 不过仍然可以将其实例作为函数的第一个参数来访问 data: vm => ({ a: vm.myProp })

## props

- 类型 Array<string> | Object
- 详细
  props 可以是数组或对象, 用于接收来自父组件的数据, props 可以是简单的数组, 或者使用对象作为替代, 对象允许配置高级选项
  基于对象语法可以使用以下选项:
  - `type`: 可以是下列原生构造函数中的一种: String、Number、Boolean、Array、Object、Date、Function、Symbol、任何自定义构造函数、或上述内容组成的数组
  - `default`: any 为该 props 指定一个默认值 对象或数组的默认值必须从一个工厂函数返回
  - `required`: Boolean 定义该 prop 是否是必填项, 在非生产环境中, 如果这个值为 truthy 且该 prop 没有被传入, 则会抛出一个控制台警告
  - `validator`: Function 自定义验证函数会将该 prop 的值作为唯一的参数带入 非生产环境中 该函数返回一个 falsy 的值 会抛出一个控制塔警告
- 实例

  ```js
  // 简单语法
  Vue.component('props-demo-array', {
    props: ['size']
  })

  // 对象语法
  Vue.component('props-demo-object', {
    props: {
      // 类型检测
      height: Number
      // 类型检测 + 其他验证
      age: {
        type: Number,
        required: true,
        validator: function(value) {
          return value >= 0
        }
      }
    }
  })
  ```

## propsData

- 类型 { [key: string]: any}
- 限制 只用于 `new` 创建的实例中
- 详细: 创建实例时传递 porps
- 示例:

  ```js
  var Comp = Vue.extend({
    props: ['msg'],
    template: `<div>{{ msg }}</div>`
  })

  var vm = new Comp({
    propsData: {
      msg: 'hello'
    }
  })
  ```

## compouted

- 类型 { [key:string]: Function | {get: Function, set: Function}}
- 详细:
  计算属性将被混入到 Vue 实例中, 所有 getter 和 setter 的 this 上下文自动的绑定为 Vue 实例
  如果使用了箭头函数, 则 this 不会指向这个组件的实例, 不过仍然可以将其实例作为函数的第一个参数来访问

  ```js
  computed: {
    aDouble: vm => vm.a * 2
  }
  ```

  计算属性的结果会被缓存, 除非依赖的响应式属性变化才会重新计算

- 示例

  ```js
  var vm = new Vue({
    data: { a: 1 },
    computed: {
      // 仅读取
      aDouble: function() {
        return this.a * 2
      }
      // 读取和设置
      aPlus: {
        get: function() {
          return this.a + 1
        },
        set: function(v) {
          this.a = v - 1
        }
      }
    }
  })

  vm.aPlus   // => 2
  vm.aPlus = 3
  vm.a       // => 2
  vm.aDouble // => 4
  ```

## methods

- 类型 { [key: string]: Function}
- 详细
  methods 将被混入到 Vue 实例中, 可以直接通过 vm 实例访问这些方法, 或在指令表达式中使用, 方法中的 this 自动绑定为 Vue 实例
- 示例

  ```js
  var vm = new Vue({
    data: { a: 1 },
    methods: {
      plus: function() {
        this.a++
      }
    }
  })
  vm.plus()
  vm.a // 2
  ```

## watch

- 类型 { [key:string]: string | Function | Object | Array}
- 详细
  一个对象, 键是需要观察的表达式, 值是对应的回调函数. 值也可以是方法名, 或者包含选项的对象, Vue 实例将会在实例化时调用 `$watch()`, 遍历 watch 对象的没一个属性
- 示例
  ```js
  var vm = new Vue({
    data: {
      a: 1,
      b: 2,
      c: 3,
      d: 4,
      e: {
        f: {
          g: 5
        }
      }
    },
    watch: {
      a: function(val, oldVal) {
        console.log('new: %s, old: %s', val, oldVal)
      },
      // 方法名
      b: 'someMethod',
      // 该回调会在任何被侦听的对象的 property 改变时被调用，不论其被嵌套多深
      c: {
        handler: function(val, oldVal) {},
        deep: true
      },
      // 该回调将会在侦听开始之后被立即调用
      d: {
        handler: 'someMethod',
        immediate: true
      },
      e: [
        'handle1',
        function handle2(val, oldVal) {},
        {
          handler: function handle3(val, oldVal) {}
        }
      ]
      // watch vm.e.f's value: {g:5}
      'e.f': function (val, oldVal) { /* ... */ }
    }
  })
  vm.a = 2 // => new: 2, old: 1
  ```
