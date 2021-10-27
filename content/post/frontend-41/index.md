---
title: '毎日のフロントエンド　41'
date: 2021-10-27T14:51:33+09:00
description: frontend 每日一练
image: frontend-41-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第四十一日

## HTML

### **JavaScript** webSocket 怎么兼容处理

`WebSocket`是一种在服务器与客户端双向通讯的技术，使用原生的`WebSocket`可以最小化 服务器资源的使用并且为两者提供了一种统一的通信方式。随着 HTML5 的普及，现代浏览器（IE10+）基本上都已经原生支持`WebSocket`

1. SockJS
2. Socket.IO

Socket.IO 能够启用基于事件的双向通信，使用它同样也需要搭建相应的服务端；首先它也会首选 WebSocket，如果不支持则会使用下面的替代方案：

- `Adobe Flash Socket`（缺点：需要在服务器上打开一个额外的端口，默认为 10843）
- `Ajax long polling`
- `Ajax multipart streaming`
- `Forever iframe`
- `JSONP polling`

## CSS

### **JavaScript** 怎么让英文单词的首字母大写

```css
/* Keyword values */
text-transform: capitalize; /*强制每个单词的首字母转换为大写*/
text-transform: uppercase; /*强制所有字符被转换为大写*/
text-transform: lowercase;
text-transform: none; /*阻止所有字符的大小写被转换*/
text-transform: full-width; /*强制字符 — 主要是表意字符和拉丁文字 — 书写进一个方形里，并允许它们按照一般的东亚文字（比如中文或日文）对齐*/
```

---

```css
.demo::first-letter {
  text-transform: uppercase;
}
```

## JavaScript

### **JavaScript** 说说对 `IIFE` 的理解

`IIFE`：立即调用函数表达式，在一些常见的框架中，会使用立即执行函数形成一个独立作用域，在这个函数通常会写一些依赖环境之类的东西；立即执行函数中，写完其中的变量不会被销毁，形成闭包

在立即执行函数中，如果想要访问全局中的变量，直接行参引入 `window` 即可

`(function(){})();`
`(function(){}());`

```js
(function () {
  statements;
})();
```

- 包围在`()`里的一个匿名函数，拥有独立的词法作用域
- 再次使用`()`创建了一个立即执行函数表达式，到此直接执行函数

## Reference

[haizlin/fe-interview: HTML/CSS/JavaScript/Vue/React/Nodejs/TypeScript/ECMAScritpt/Webpack/Jquery/小程序/软技能……](https://github.com/haizlin/fe-interview)

[WebSocket 教程 - 阮一峰的网络日志](https://www.ruanyifeng.com/blog/2017/05/websocket.html)
