---
title: '毎日のフロントエンド　13'
date: 2021-09-27T21:33:31+09:00
description: frontend 每日一练
image: frontend-13-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第十三日

## HTML

### **#Question:** html5 中的 `form` 怎么关闭自动完成？

操作表单 `form` 的 `autocomplete` 属性值, 默认是开启的。

```html
<form action="demo_form.html" method="get" autocomplete="off">
  First name:<input type="text" name="fname" /><br />
  E-mail: <input type="email" name="email" /><br />
  <input type="submit" />
</form>
```

## CSS

### **#Question:** `::before`和`:after`中单冒号和双冒号的区别是什么，这两个伪元素有什么作用

1. `:`表示伪类，是一种样式，比如`:hover`, `:active` 等

2. `::`表示伪元素，是具体的内容，比如`::before` 是在元素前面插入内容，`::after` 则是在元素后面插入内容，不过需要 `content` 配合，并且插入的内容是` inline` 的

3. `:before` 和 `:after` 其实还是表示伪元素，在 css3 中已经修订为`::before` 和`::after` 了，只是为了能兼容 IE 浏览器，所以也可以表示成`:before` 和`:after`

## JavaScript

### **#Question:** 说说你对 javascript 的作用域的理解

作用域就是一块封闭的区域，外部不能访问到这块区域里的变量、函数等，被{}包裹的就叫一个作用域（全局作用域是特殊的）

- 函数作用域
- 全局作用域
- 块作用域

**变量提升**：在用 var 或者 function 声明一个变量和函数时，变量和函数会被提升到函数的顶部

## Soft Skills

### **#Question:** http 都有哪些状态码

[HTTP 状态码 对照详解](https://tool.oschina.net/commons?type=5)

## Reference

[css 伪元素:before 和:after 用法详解](https://www.cnblogs.com/wonyun/p/5807191.html)

[前端面试每日 3+1-以前端面试题来驱动学习，提倡每日学习与思考，每天进步一点！](http://www.h-camel.com/index.html)
