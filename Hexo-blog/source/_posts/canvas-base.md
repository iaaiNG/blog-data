---
title: 关于Canvas的基础
categories: 
- 技术
tags:
- HTML5
- Canvas
---

Canvas 是 HTML5 新增的，一个可以使用脚本(通常为JavaScript)在其中绘制图像的 HTML 元素。它可以用来制作照片集或者制作简单(也不是那么简单)的动画，甚至可以进行实时视频处理和渲染。
<!-- more -->
## canvas标签
```html
<canvas width="500" height="400">
    您的浏览器版本太低，请更新!
</canvas>
```
* canvas标签在浏览器中默认`300*150`的`inline-block`
* 画布的宽度和高只能使用`html/js`属性来赋值
 
## 画笔对象
```js
var ctx = canvas.getContext("2d");
```
* 每个画布上有且只有一个“画笔”对像，该对象进行绘图

## Main API
| API              | 描述           | 
| -----------------|--------------- |
| ctx.lineWidth  = 1         | 描边宽度(空心)        | 
| ctx.fillStyle = "#000"        | 填充样式/颜色(实心)         | 
| ctx.strokeStyle="red"         | 描边样式(空心)         |
|tx.fillRect(x,y,w,h)|      填充一个矩形|
|ctx.clearRect(x,y,w,h)   | 清除一个矩形范围内所有绘图|

## 矩形绘制
| API              | 描述           | 
| -----------------|--------------- |
|ctx.strokeRect(x,y,w,h) |  描边一个矩形|

## 文本绘制
| API              | 描述           | 
| -----------------|--------------- |
|ctx.textBaseline = "top/bottom/alphabetic"|   #文本基线:(垂直对齐方式)|
|ctx.font = "12px sans-serif"| 字体大小和字体|
|ctx.fillText(str,x,y)  |       填充一段文本| 
|ctx.strokeText(str,x,y)  |    描边一段文本| 
|ctx.measureText(str)  |    测量文本宽度,返回对象{width:x}| 

## 添加渐变对象
>线性渐变:linearGradient
>径向渐变:radialGradient

* 创建渐变对象
```js
 var g = ctx.createLinearGradient(x1,y1,x2,y2);
```
* 设置颜色点
```js
g.addColorStop(0,"blue");
g.addColorStop(0.5,"red");
g.addColorStop(1,"green");
```
* 将渐变对象应用填充或描边样式
```js
ctx.fillStyle = g;
ctx.strokeStyle = g;
```
## 路径绘制
>Path：由多个坐标点组成任意形状,路径不见，可用于"描边"，"填充"，"裁剪";

  | API              | 描述           | 
| -----------------|--------------- |
 | ctx.beginPath()  |  		开始一条新路径|
 | ctx.closePath()     |		闭合当前路径|
 | ctx.moveTo(x,y)     |		移动指定点 |
 | ctx.lineTo(x,y)       |		从当前点到指定点画一条直线|
  |ctx.arc(cx,cy,r,start,end)  |	绘制圆拱形路径|
 | ctx.stroke()     |       对当前路径描边|
 | ctx.fill()          |      	对当前路径填充|
 | ctx.clip()          |      对当前路径裁剪|
 
## 图像绘制
> canvas属于客户端技术，图片在服务器上，所以浏览器必须先下载绘制图片，且等待图片加载成功后再绘制图片

```js
var p3 = new Image();   //1:创建图片对象
p3.src = "img/p3.png";  //2:发送请求并且下载图片
p3.onload = function(){  //3:图片下载完成，触发事件onload
  ctx.drawImage(p3,x,y);     //原始大小绘图
  ctx.drawImage(p3,w,y,w,h); //拉伸绘图
}
```

## 变形操作
> Canvas绘图中也变形技术，可以针对一个图像/图形绘制过程进行变形, rotate; translate;

  | API              | 描述           | 
| -----------------|--------------- |
 | ctx.rotate(弧度)  | 旋转绘图对象，轴点画布原点(0,0)|
 | ctx.translate(x,y) |  整个画布的原点平移到指定的点|
 | ctx.save()   |    保存画笔所有变形状态值|
 | ctx.restore()  |   恢复画布变形状态值到最近一次保存|