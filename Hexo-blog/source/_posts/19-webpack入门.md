---
title: webpack入门
date: 2019-04-11 10:03:50
category: 前端技术
---

我的理解：webpack 可以让项目可以分模块构建，增强模块独立性，按需求引入依赖，更适合大型项目。
本质上，webpack 是一个现代 JavaScript 应用程序的静态模块打包器(module bundler)。当 webpack 处理应用程序时，它会递归地构建一个依赖关系图(dependency graph)，其中包含应用程序需要的每个模块，然后将所有这些模块打包成一个或多个 bundle。
<!--more-->



# 一、安装与使用
## (1) 本地安装
```
npm install -S webpack
```
在 webpack 4+ 版本，还需要安装 CLI
```
npm install -S webpack-cli
```
## (2) 使用
通过在 package.json 配置一个 scripts，会在本地 node_modules 目录中查找安装的 webpack
```
"scripts": {
  "start": "webpack --config webpack.config.js"
}
```
执行 `npm start` 使用 webpack 工具。
### webpack 常用指令
- `--config` 指定配置打包的文件
- `--progess` 查看打包过程
- `--display-reasons` 显示打包原因
- `--display modules` 显示打包模块
- `--hide modules` 隐藏打包模块
- `--watch` 监听目录，随时打包
- `--module-bind 'css=style-loader!css-loader'` 指定模块打包方式



# 二、配置 webpack.config.js
## (1) 方式
1. 终端
2. Node.js (推荐)
## (2) Node.js 环境下配置 webpack.config.js
*参考：https://www.webpackjs.com/configuration/*
```js
// webpack.config.js
const path = require('path');

module.exports = {
  // 内置优化模式
  mode: "production", // "production" | "development" | "none"

  // 上下文
  context: __dirname, // string 
  // webpack 的主目录，默认使用当前目录（绝对路径）
  // 用于从配置中解析 entry 和 module.rules.loader 选项

  // 构建目标 
  target: "web", // 默认"web"

  // 入口
  entry: { // string | object | array
    a: "./app/entry-a",
    b: ["./app/entry-b1", "./app/entry-b2"]
  },

  // 出口
  output: { // webpack 如何输出结果的相关选项
    path: path.resolve(__dirname, "dist"), // string 所有输出文件的目标路径，必须是绝对路径
    filename: "[name].[chunkhash].bundle.js", // string
    publicPath: "https://cdn.example.com/", // string 输出解析文件的目录，url 相对于 HTML 页面
  },

  //模块配置
  module: { // 关于
    rules: [ // 模块规则
      {
        test: /\.jsx?$/,  // 推荐匹配文件名
        include: [
          path.resolve(__dirname, "app")
        ],
        exclude: [
          path.resolve(__dirname, "app/demo-files")
        ],
        // 这里是匹配条件，每个选项都接收一个正则表达式或字符串
        // test 和 include 具有相同的作用，都是必须匹配选项
        // 推荐在 include 和 exclude 中绝对路径数组
        // exclude 是必不匹配选项（优先于 test 和 include）
        // - 尽量避免 exclude，更倾向于使用 include

        loader: "babel-loader",
        // 应该应用的 loader，它相对上下文解析
        // 为了更清晰，`-loader` 后缀在 webpack 2 中不再是可选的

        options: { // loader 的可选项
          presets: ["es2015"]
        },
      },
      {
        test: "\.html$",
        use: [ // 应用多个 loader 和选项
          "htmllint-loader",
          {
            loader: "html-loader",
          }
        ]
      },
    ],

    noParse: [ // 不解析这里的模块
      /special-library\.js$/
    ],
  },

  // 附加插件列表
  plugins: [
    // ...
  ],
}
```

# 三、入口 Entry
## (1) 单个入口
单个入口往往输出单个出口
### 传入字符串
```js
module.exports = {
  entry: "./index.js"
}
\\ 其实是下面方式的简写
module.exports = {
  entry: {
    main: './index.js'
  }
}
```
### 传入数组
将数组里的入口模块打包成一个文件
```js
module.exports = {
  entry: ["./index.js", "./app.js]
}
\\ 其实是下面方式的简写
module.exports = {
  entry: {
    main: ["./index.js", "./app.js]
  }
}
```
## (2) 多个入口
多个入口往往需要配置多个出口
```js
module.exports = {
  entry: {
    page1: './index.js'
    page2: './app.js'
  }
}
```

# 四、出口 Output
## (1) 单个出口
```js
module.exports = {
  output: {
    filename: "bundle.js"  // 文件名
    path: __dirname // 路径
  }
}
```
## (2) 多个出口
- `[name]`: 使用入口名字
- `[hash]`: 使用每次构建过程中，唯一的 hash 生成
- `[chunkhash]`: 使用基于每个 chunk 内容的 hash
```js
module.exports = {
  output: {
    filename: "[name]-[hash]-[chunkhash].js"  // 文件名
    path: __dirname  // 路径
  }
}
```


# 五、加载器 Loader
webpack 可以使用 loader 来预处理文件。这允许你打包除 JavaScript 之外的任何静态资源。
## (1) 配置 loader 方式
### 配置：在 webpack.config.js 文件中指定 loader。
```js
// webpack.config.js
module.exports = {
  module: {
    rules: [
      { test: /\.css$/, use: 'css-loader' },
      { test: /\.ts$/, use: 'ts-loader' }
    ]
  }
};
```
### 内联：在每个 import 语句中显式指定 loader。
```js
import Styles from 'style-loader!css-loader?modules!./styles.css';
```
### CLI：在 shell 命令中指定它们。
```
webpack --module-bind jade-loader --module-bind 'css=style-loader!css-loader'
```
## (2) css-loader
css-laoder 可以加载 JS 中引入的 css 文件
```
import css from 'file.css';
```
### 常用选项
- importLoaders： 用于配置 css 文件中 @import 多少个样式。

## (3) style-loader
把 css 以 `<style>` 标签的形式插入 dom




# 六、插件 Plugins
插件目的在于解决 loader 无法实现的其他事
## (1) html-webpack-plugin 插件
### 作用
该插件将为你生成一个 HTML5 文件， 其中包括使用 script 标签的 body 中的所有 webpack 包。这对于在文件名中包含每次会随着编译而发生变化哈希的 webpack bundle 尤其有用。
### 配置方式
```js
plugins: [
    new htmlWebpackPlugin({})  // 调用插件
]
```
### 常用配置
- **filename**: HTML文件名
- **template**: HTML模板文件
- **inject**: 定义生成的 script 标签位置，false则不注入
- **minify**: 定义压缩规则,
- **chunks**: 允许仅添加一些chunks
### HTML模板传参
- 自定义配置参数，如：`title: 'hello webpack'`
- HMTML模板文件使用：`<body><%= htmlWebpackPlugin.options.title %></body>`
## html-webpack-inline-source-plugin 插件
给 html-webpack-plugin 插件传入对象添加 inlineSource 属性，使 JS 和 CSS 内联插入HTML


