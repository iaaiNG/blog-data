---
title: 关于webpack配置的理解
category: 技术
tag: 
- Webpack
---
WebPack可以看做是**模块打包机**：它做的事情是，分析你的项目结构，找到JavaScript模块以及其它的一些浏览器不能直接运行的拓展语言（Scss，TypeScript等），并将其转换和打包为合适的格式供浏览器使用。
<!-- more -->

>vue + webpack 打造todo应用
>[依赖版本参考](https://blog.csdn.net/yangluan999/article/details/79980275)

## vue-loader+webpack项目配置
```json
//package.json
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build":"webpack --config webpack.config.js",
}
```
```html
<!-- app.vue -->
<template>
  <div class="text">{{text}}</div>
</template>

<script>
export default {
  data(){
    return {
      text:'jkm'
    }
  }
}
</script>

<style>
.text{
  color: aqua;
  margin-left:50px;
}
</style>
```
```javascript
//index.js
import Vue from 'vue'
import App from './app.vue'

const root = document.createElement('div')
document.body.appendChild(root)

new Vue({
  render:(h)=>h(App)
}).$mount(root)
```
```javascript
// webpack.config.js
const path = require('path')  

const config = {
  //入口路径
  entry: path.join(__dirname, 'src/index.js'),
  //出口路径
  output: {
    filename: 'bundle.js',
    path: path.join(__dirname, 'dist')
  },
  //由于webpack本身只支持js和es5的语法，不理解vue语法，为此在rules下新增规则
  module: {
    rules: [
      {
        test: /\.vue$/,		
        loader: 'vue-loader'
      }
    ]
  },

module.exports = config
```
## webpack配置项目加载各种静态资源及css预处理器
```javascript
{
  test: /\.css$/,
  use: [
    'style-loader',  //把css代码以js的方式写到html里面
    'css-loader'  //读取css文件的样式
  ]
},
{
  test: /\.(gif|jpg|jpeg|png|svg)$/,
  use: [
    {
      loader: 'url-loader',  //把图片转换成base64格式，写在出口文件bundle.js里
      options: {
        limit: 1024,  //限制图片的大小
        name: '[name]-jkm.[ext]'  //输出图片的名字
      }
    }
  ]
},
{
  test: /\.styl/,
  use: [
    'style-loader',
    'css-loader',
    'stylus-loader'
  ]
}
```
## webpack-dev-server的配置和使用
```json
//package.json
1、设置环境变量 NODE_ENV = productio/development；
2、为适应不同平台（windows、linux、mac...），利用cross-env来帮助读取。
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build":"cross-env NODE_ENV=production webpack --config webpack.config.js",
    "dev":"cross-env NODE_ENV=development webpack-dev-server --config webpack.config.js"
}
```
```javascript
// webpack.config.js
const path = require('path')  
const HTMLPlugin = require('html-webpack-plugin')   //帮助生成html文件，并且自动包含bundle.js
const webpack = require('webpack')

const isDev = process.env.NODE_ENV === 'development'  //NODE_ENV保存在process.env对象中

const config = {
	target:'web'  //设置编译目标为web平台
	...
	plugins: [
	  //可以让webpack编译过程中，以及自己的js代码中，调用 process.env.NODE_ENV 来判断环境
	  new webpack.DefinePlugin({
	    'process.env': {
	      NODE_ENV: isDev ? '"development"' : '"production"'
	    }
	  }),
	  new HTMLPlugin()
	]
}
if (isDev) {
  config.devtool = "#cheap-module-eval-source-map"  //帮助在浏览器调试代码，把webpack编译后的代码，通过代码的映射，转换成自己的代码。
  config.devServer = {   //
    port: 8000,  //端口
    host: '0.0.0.0',  //能同时通过localhost、127.0.0.1、本机内网IP访问
    overlay: {  //可以把webpack编译过程中出现的错误，显示到网页中
      errors: true,
    },
    hot: true,  //修改代码中数据保存后，只会重新渲染修改组件的数据，不会让整个页面重新加载。
    open:true  //自动打开浏览器
  }
  config.plugins.push(
    new webpack.HotModuleReplacementPlugin(),   //使支持hot功能
    new webpack.NoEmitOnErrorsPlugin()	//减少不需要信息的展示的问题
  )
}
module.exports = config
```