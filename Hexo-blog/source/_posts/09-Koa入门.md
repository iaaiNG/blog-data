---
date: 2018/9/09 16:46:25
title: koa 入门
categories: 
- 后端技术
tags:
- koa
---

Koa 是一个新的 web 框架，由 Express 幕后的原班人马打造， 致力于成为 web 应用和 API 开发领域中的一个更小、更富有表现力、更健壮的基石。 通过利用 async 函数，Koa 帮你丢弃回调函数，并有力地增强错误处理。
<!--more-->
## 在 node < 7.6 版本的 Koa 中使用 ES7 async 方法
1. 安装shell
```
npm install --save koa
cnpm install babel babel-register babel-preset-env --save
```
- 配置入口文件 app.js
```
require("babel-register")
require('./server.js')
```
- 配置 .babelrc 文件
```
{ "presets": [
  ["env", {
    "targets": {
      "node": true
      }
    }
  ]
]}
```
- Koa 入门，配置 server.js
```
var koa = require('koa');
var app = new koa();
app.use(async(ctx, next) => {
  ctx.body = 'hello koa'
});
app.listen(8080);
```