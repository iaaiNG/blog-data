---
title: 关于 JavaScript 历代目的记录
categories: 
- 前端技术
tags:
- javascript
---

就基于javascript技术，ES6开始的内容，做一些总结，以方便快速开发。
<!--more-->
# ES6

## Promise
 
```javascript
var promise = new Promise(function(resolve, reject) {
  // ... some code
  if (/* 异步操作成功 */){
    resolve(value);
  } else {
    reject(error);
  }
});
```
可以链式调用：`promise().then().catch()`
虽然Promise解决了深度回调的问题，但是却无法避免代码的层叠嵌套:astonished:。
# ES7
## async/await
- async顾名思义是“异步”的意思，async用于声明一个函数是异步的。而await从字面意思上是“等待”的意思，就是用于等待异步完成。并且await只能在async函数中使用。
- 通常async、await都是跟随Promise一起使用的。为什么这么说呢？因为async返回的都是一个Promise对象同时async适用于任何类型的函数上。而await紧跟一个Promise直接是取得异步的结果。
 
```
async function dosth(){
  let response = await promise
}
dosth()
```
async、await也使用到了Promise但是却减少了Promise的then处理使得整个异步请求代码优雅了许多。