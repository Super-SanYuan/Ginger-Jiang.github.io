---
title: vue安装使用
date: 2020-02-02 19:32:47
tags:
  - Vue
categories:
  - Vue
---

## Vue Devtools

[Devtools](https://github.com/vuejs/vue-devtools#vue-devtools) 是一个 Vue 官方提供的浏览器插件, 他可以让我们在开发过程中国更直观更友好的的在浏览器中对 Vue 项目进行审查和调试

### 版本

Vue 分为开发版本和生产版本

- 开发版本 - 包含完整的警告和调试模式
- 生产版本 - 删除了警告

## 使用

### 直接引入(本地文件或 CDN)

在 html 文件中, 我们如果需要使用 Vue 可以直接使用 script 标签进行引入

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
  </head>
  <body>
    <!-- 最新版本 适合开发环境 -->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <!-- 指定版本 适合生产环境追溯问题 -->
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.11"></script>
  </body>
</html>
```

### NPM

在使用 Vue 开发大型应用时, 推荐使用 NPM 安装, NPM 能够很好的与 webpack 配合使用.

```Shell
$ npm install vue
```

### 命令行工具

```shell
vue create hello-word
```

Vue 提供了一个官方的 Cli, 为单页面应用(SPA)快速搭建繁杂的脚手架.

- [Cli GitHub](https://github.com/vuejs/vue-cli)
- [Cli 文档](https://cli.vuejs.org/zh/)
