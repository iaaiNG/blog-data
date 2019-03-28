---
date: 2018/2/07 16:46:25
title: css基础
categories: 前端技术
tags:
- CSS
---
css全称叫Cascading Style Sheet，可译为“层叠样式表”或“级联样式表”，用于控制Web页面的外观。通过使用CSS样式设置页面的格式，可将页面的内容与表现形式分离。
<!--more-->

# CSS的语法规范
## 使用方式
1. 内联方式: 
```html
<div style="background:red;"></div>
```
2. 内部样式表: 
```css
<style>
  .container{
    background: blue;
  }
</style>
```
3. 外部样式表
```html
<link rel="stylesheet" href="index.css">
```
## 特征
### 继承性
大部分的css效果是可以直接继承给子元素的。
### 层叠性
一个元素定义多个样式规则，规则中的属性不冲突时，可以同时都应用到当前元素上。
### 优先级
在层叠性基础上，如果样式声明冲突的话,则按照默认的优先级去应用样式。
  * 浏览器默认设置
  * 样式表(内外部) 就近原则 - 后定义者优先
  * 内联方式
  * !important
  **优先级由低到高**
## 选择器
selectors|syntax
-|-
通用选择器|`* { }`
元素选择器|`div { }`
类选择器|`.class-name { }`
ID选择器|`#id-name { }`
群组选择器|`div, .class-name, #id-anme { }`
后代选择器|`#id-anme .class-name div { }`
子代选择器|`.class-name>div { }`
<table>
<tr>
  <th>selectors</th>
  <th>syntax</th>
  <th>description</th>
</tr>
<tr>
  <td rowspan="5">伪类选择器</td>
  <td>a:link { }</td>
  <td>匹配元素a尚未被访问时候的状态</td>
</tr>
<tr>
  <td>a:visited { }</td>
  <td>匹配元素a访问过的状态</td>
</tr>
<tr>
  <td>:hover { }</td>
  <td>鼠标悬停时的效果</td>
</tr>
<tr>
  <td>:active { }</td>
  <td>鼠标保持按下时的状态</td>
</tr>
<tr>
  <td>:focus { }</td>
  <td>获取焦点时的状态</td>
</tr>
</table>

# 单位
## 尺寸单位
`px, in(英寸), pt(磅), cm, mm, em, rem`
## 颜色单位
`rgb, rgba, #rrggbb(位十六进制的数字), #rgb(缩写), yellow...`

# 尺寸
## 语法
attribute|description
-|-
width | 宽度
min-width | 最小宽度
max-width | 最大宽度
height | 高度
min-height | 最小高度
max-height | 最大高度
## 允许设置尺寸的元素
1. 所以块级元素
2. 部分行内块元素:  `radio, checkbox`
3. 本身具备width, height的元素
## 溢出处理
<table>
  <tr>
    <th>attribute</th>
    <th>value</th>
    <th>description</th>
  </tr>
  <tr>
    <td rowspan="4">overflow <br><br> overflow-x <br><br>overflow-y</td>
    <td>visible</td>
    <td>默认值，溢出可见</td>
  </tr>
  <tr>
    <td>hidden</td>
    <td>溢出隐藏</td>
  </tr>
  <tr>
    <td>scroll</td>
    <td>显示滚动条，溢出可用</td>
  </tr>
  <tr>
    <td>auto</td>
    <td>溢出时才显示滚动条</td>
  </tr>
</table>

# 边框

<table>
  <tr>
    <th>attribute</th>
    <th>value</th>
    <th>description</th>
  </tr>
  <tr>
    <td>border</td>
    <td>width style color</td>
    <td>边框</td>
  </tr>
  <tr>
    <td>border-top/right/bottom/left</td>
    <td>width style color</td>
    <td>单边框</td>
  </tr>
  <tr>
    <td>border-radius</td>
    <td>px, %</td>
    <td>边框倒角</td>
  </tr>
  <tr>
    <td rowspan="6">box-shadow<br>边框阴影</td>
    <td>h-shadow</td>
    <td>*阴影的水平偏移距离</td>
  </tr>  
  <tr>
    <td>v-shadow</td>
    <td>*阴影的垂直偏移距离</td>
  </tr>    
  <tr>
    <td>blur</td>
    <td>模糊距离</td>
  </tr> 
  <tr>
    <td>spread</td>
    <td>边框属性阴影的大小，指定要在基础阴影上扩充出来的大小距离</td>
  </tr> 
  <tr>
    <td>color</td>
    <td>阴影颜色</td>
  </tr> 
  <tr>
    <td>inset</td>
    <td>内阴影</td>
  </tr> 
  <tr>
    <td>outline</td>
    <td>width style color; none/0 为取消轮廓</td>
    <td>轮廓</td>
  </tr> 
</table>

# 框模型

