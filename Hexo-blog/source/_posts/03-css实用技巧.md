---
date: 2018/3/06 16:46:25
title: css 实用技巧
categories: 前端技术
tags:
- CSS
---

好记性不如烂笔头，能写下来的绝对不会记的<(￣︶￣)>，这里专门收录一些CSS常用的小技巧
<!-- more -->
# 文本溢出显示省略号
## 单行
```
overflow:hidden;
text-overflow:ellipsis;
white-space:nowrap
```
## 多行
只有-webkit内核才有作用
```
overflow: hidden;
text-overflow: ellipsis;
display: -webkit-box;
-webkit-line-clamp: 2;
-webkit-box-orient: vertical;
```
|属性|作用|
|--|--|
|-webkit-line-clamp  | 用来限制在一个块元素显示的文本的行数,这是一个不规范的属性（unsupported WebKit property），它没有出现在 CSS 规范草案中 |
|  display: -webkit-box| 将对象作为弹性伸缩盒子模型显示  |
|-webkit-box-orient  |设置或检索伸缩盒对象的子元素的排列方式 。| 
|text-overflow: ellipsis| 以用来多行文本的情况下，用省略号“…”隐藏超出范围的文本。|



# 实现文字两端对齐
## 写法一（缺点兼容性差）
```css
div {
  width: 100px;
  border: 1px solid red;
  text-align: justify;
  text-align-last: justify;
  text-justify: distribute;
}
```
## 写法二（引入伪类）
text-align: justify; 属性可单独使用，前提是文本需要超过两行，不过最后一行不会两边对齐。因此如果是单行，可使用 hack 来“伪造”最后一行，以达到单行视觉上的两边对齐效果。
```css
div{
  width:500px;
  text-align: justify;
}
div:after {
    content: " ";
    display: inline-block;
    width: 100%;
}
```
# CSS 移动端适配
## 利用手机淘宝，rem适配方案 https://github.com/amfe/lib-flexible
安装
```
npm i -S amfe-flexible
```
嵌入网页
```html
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no"><script src="./node_modules/amfe-flexible/index.js"></script>
```
## 了解rem和em 
- rem是基于html元素的字体大小来决定，而em则根据使用它的元素的大小决定
- 使用 em 单位的唯一原因是缩放没有默认字体大小的元素，比如按钮，菜单和标题可能会有自己明确的字体大小。当你修改字体大小的时候，希望整个组件都适当缩放
