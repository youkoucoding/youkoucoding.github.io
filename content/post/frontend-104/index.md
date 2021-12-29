---
title: '毎日のフロントエンド  104'
date: 2021-12-29T20:38:33+09:00
description: frontend 每日一练
image: frontend-104-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第一百零四日

## HTML

### **Question:** `<html>`标签有什么作用

`<html>` 元素定义了整个 HTML 文档。

- 告诉浏览器这是一个 HTML 文档
- 标识了文档开始和结束的位置
- 最外层的标签

> - html `document.documentElement`
> - head `document.head`
> - body `document.body`
> - 所有的 script 对应 js 的 `document.scripts`
> - 所有的 img 对应 js 的 `document.imgs`

## CSS

### **Question:** 行内元素和块级元素有什么区别，如何相互转换

#### 块级元素：

- 独占一行,默认情况下，其宽度自动填满其父元素宽度
- 可以设置`margin`和`padding`属性
- 可以设置`width`，`height`属性

#### 行内元素：

- 相邻的行内元素会排列在同一行里，直到一行排不下，才会换行，其宽度随元素的内容而变化
- 行内元素起边距作用的只有`margin-left`、`margin-right`、`padding-left`、`padding-right`，其它属性不会起边距效果。
- **不可置换行内元素**不能设置`width`、`height`和垂直方向上的 margin，而**可置换行内元素**(`<img>`，`<input>`等)则可以。
- 行内元素又分为可置换元素与不可置换元素。可置换元素展示的内容是通过元素的 `src`、`value` 等属性或 CSS content 属性从外部引用得到的，可被替换的，如`<img>`，`<input>`, `<textarea>`等。剩下的就是不可置换元素。

> 通过`display: block;` `display: inline;` 来转换

## JavaScript

### **Question:** `json`和`jsonp`的区别

- json 是一种数据结构
- jsonp 是一种用于跨域的技术，JSON with Padding 的略称，是 json 的一种‘使用模式’，可以跨域的获取到数据
  - script 标签可以实现跨域，而且可以跨域执行 js 文件，因此可以插入 script 标签来引入 js 文件，通过动态生成 js 文件，客户端通过执行 js 文件中的函数，获取返回值来得到需要的数据。
  - 优点是兼容性较佳，支持服务器和浏览器的双向通信，缺点是只支持 get 请求。

### **Question:** 如下代码的返回值

```js
String('11') == new String('11');
String('11') === new String('11');
```

Result: `true` `false`

- `new String()` 返回的是对象
- `==` 隐式转换，实际运行的是 String('11') == new String('11').toString();
- `===` 类型不一样，一个是 string，一个是 object, `String()` 返回的是字符串

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[前端进阶](https://muyiy.cn/)

[前端布局基础概述 - 掘金](https://juejin.cn/post/6844903583331123214)

[一分钟说完 JSONP 请求 - 掘金](https://juejin.cn/post/6844903976505344013)

[面试题之 Jsonp 的理解及手写代码 - 掘金](https://juejin.cn/post/6947694608247685127)
