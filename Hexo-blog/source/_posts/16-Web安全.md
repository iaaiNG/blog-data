---
title: Web安全
date: 2019-04-02 17:14:49
category: 网络安全
---
安全是一个非常重要的话题，非常有必要了解其攻击的原理和防范的方法。
<!--more-->
# 一、为什么要用 HTTPS
## HTTP 劫持
- HTTP 协议没有办法对通信对方的身份进行校验以及对数据完整性进行校验。  
- 例如：网络运营商在正常HTTP请求的数据流中插入精心设计的网络数据报文，让客户端展示“错误”的数据。

## HTTPS 优势
- 使用HTTPS协议可认证用户和服务器，确保数据发送到正确的客户机和服务器；
- HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，要比http协议安全，可防止数据在传输过程中不被窃取、改变，确保数据的完整性。

# 二、XSS(跨站脚本攻击) 和 X-XSS-Protection
**XSS：** (Cross Site Scripting)为不和层叠样式表(CSS)的缩写混淆，故将跨站脚本攻击缩写为XSS，XSS是攻击者利用漏洞，向 Web 页面中注入恶意代码，当用户浏览该页之时，注入的代码会被执行，从而达到攻击的特殊目的。  
**X-XSS-Protection：** HTTP X-XSS-Protection 标头是IE，Chrome和Safari的一个功能，当检测到跨站脚本攻击 (XSS)时，浏览器将停止加载页面。

# 三、CSP
CSP(Content Security Policy)内容安全策略。这个规范与内容安全有关，主要是用来定义页面可以加载哪些资源，减少 XSS 的发生。

## CSP白名单
- 作用：告诉客户端允许加载和不允许加载的内容，定义此政策后，浏览器加载来自任何其他来源的脚本，只会引发一个错误。
- 引入方法
  -  使用meta标签， 直接在页面添加meta标签，
      ```html
      <meta http-equiv="Content-Security-Policy" content="script-src 'self' https://host1.com https://host2.com">
      ```
      点评：每个页面都需要添加，而且不能对限制的域名进行上报，但优先级高。
  - 在服务端配置csp
      ```shell
      Nginx :
      In your server {} block add:
      add_header Content-Security-Policy "default-src 'self';";
      ```
      点评：在服务端配置所有的页面都可以不需要改了，而且还支持上报。
- 格式：HTTP标头: 指令 关键字 有效的脚本来源 有效的脚本来源...;    

## CSP指令  
- base-uri 用于限制可在页面的 <base> 元素中显示的网址。
- child-src 用于列出适用于工作线程和嵌入的帧内容的网址。例如：child-src https://youtube.com 将启用来自 YouTube（而非其他来源）的嵌入视频。
- connect-src 用于限制可（通过 XHR、WebSockets 和 EventSource）连接的来源。
- font-src 用于指定可提供网页字体的来源。
- form-action 用于列出可从 <form> 标记提交的有效端点。
- default-src 此指令用于定义您未指定的大多数指令的默认值 
- sandbox  它限制的是页面可进行的操作，而不是页面可加载的资源

## CSP关键字
- 'none' 不执行任何匹配。
- 'self' 与当前来源（而不是其子域）匹配。
- 'unsafe-inline' 允许使用内联 JavaScript 和 CSS。
- 'unsafe-eval' 允许使用类似 eval 的 text-to-JavaScript 机制。

## 内联代码和eval()被视为是有害的。
- 原因：内联代码可以带来XSS攻击。而eval()、new Function()...将不活动文本转换为可执行的 JavaScript，并代表他们执行它。
- CSP解决：完全禁止内联脚本；CSP默认阻止eval这些方法。
- 做法：
  - 内联script标记，改引入外部js文件
  - 用`addEventListener()`调用，替换元素属性定义事件
  - 不使用eval



# 四、CSRF(跨站请求伪造)
**CSRF：** 是一种冒充受信任用户，向服务器发送非预期请求的攻击方式。
## 简单例子：
假如博客园有个加关注的GET接口，blogUserGuid参数很明显是关注人Id，如下：
```
http://www.cnblogs.com/mvc/Follow/FollowBlogger.aspx?blogUserGuid=4e8c33d0-77fe-df11-ac81-842b2b196315
```
那我只需要在我的一篇博文内容里面写一个img标签：
```html
<img style="width:0;" src="http://www.cnblogs.com/mvc/Follow/FollowBlogger.aspx?blogUserGuid=4e8c33d0-77fe-df11-ac81-842b2b196315"/>
```
那么只要有人打开我这篇博文，那就会自动关注我。

## 防御CSRF攻击的手段
1. 尽量使用POST，限制GET
2. 加验证码
3. Anti CSRF Token（推荐）
   - 服务端验证表单中的Token是否与用户Session（或Cookies）中的Token一致，来判断请求是否合法

# 五、clickjacking(点击劫持) 和 X-Frame-Options
**ClickJacking：** 点击劫持，是一种视觉上的欺骗手段。  

## 两种攻击方式：
- 攻击者在原有网页覆盖一个透明的iframe，然后诱使用户在该页面上进行操作。
- 攻击者使用一张图片覆盖在网页，遮挡网页原有位置的含义；

## 解决：
使用 HTTP X-Frame-Options 标头，配置参数：
- DENY：浏览器会拒绝当前页面加载任何frame页面；
- SAMEORIGIN：frame页面的地址只能为同源域名下的页面；
- ALLOW-FROM origin：允许frame加载的页面地址；

# 参考
*【前端安全】JavaScript防http劫持与XSS：http://www.cnblogs.com/coco1s/p/5777260.html*
*CSP内容安全政策：https://developers.google.cn/web/fundamentals/security/csp/*
*Web安全之ClickJacking：https://www.cnblogs.com/lovesong/p/5248483.html*
*Web安全之CSRF攻击：https://www.cnblogs.com/lovesong/p/5233195.html*