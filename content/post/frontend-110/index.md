---
title: '毎日のフロントエンド  110'
date: 2022-01-04T11:10:29+09:00
description: frontend 每日一练
image: frontend-110-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第一百一十日

## HTML

### **Question:** `svg`有什么运用场景

```html
<svg xmlns="http://www.w3.org/2000/svg" version="1.1">
  <rect width="300px" height="100px" />
</svg>
```

SVG 是可缩放的矢量图形，是用 XML 来定义的图像。一个最基础的 svg 标签如下：

全称是可缩放矢量图（Scalable Vector Graphics）。其他图像格式都是基于像素处理的，SVG 则是属于对图像的形状描述，所以它本质上是文本文件，体积较小，且不管放大多少倍都不会失真。

- SVG 文件可以直接插入网页，成为 DOM 的一部分，然后用 JavaScript 和 CSS 进行操作。

## CSS

### **Question:** 设备像素比

设备像素比(Device Pixel Ratio 简称：`DPR`)，是指物理像素和 CSS 像素(设备独立像素)的比例。

#### `viewport`

```html
<meta
  name="viewport"
  content="width=device-width,initial-scale=1,minimum-scale=1,maximum-scale=1,user-scalable=no"
/>
```

- `width` : 控制 `viewport` 的大小，能够指定一个值，如 600， 或者特殊的值，如 device-width 为设备的宽度（单位为缩放为 100%的 CSS 的像素）
- `height` : 和 width 相对应，指定高度
- `initial-scale` : 初始缩放比例，页面第一次加载时的缩放比例
- `maximum-scale` : 容许用户缩放到的最大比例，范围从 0 到 10.0
- `minimum-scale` : 容许用户缩放到的最小比例，范围从 0 到 10.0
- `user-scalable` : 用户是否能够手动缩放，值能够是：`true`容许用户缩放；`false`不容许用户缩放

## JavaScript

### **Question:** AJAX 的工作原理

AJAX 即“Asynchronous Javascript And XML”，异步请求数据的 web 开发技术，是在不重新加载整个页面的情况下，与服务器交换数据并更新部分网页。

基本原理是，通过`XMLHttpRequest`向服务器发送异步请求，获得服务器返回的数据，利用 js 更新页面。其核心功能在于`XMLHttpRequest`对象。
创建一个 ajax 的步骤大致可以分为以下几步

- 创建`XMLHttpRequest`对象
- 打开链接 （指定请求类型，需要请求数据在服务器的地址，是否异步 i 请求）
- 向服务器发送请求（get 类型直接发送请求，post 类型需要设置请求头）
- 接收服务器的响应数据（需根据 XMLHttpRequest 的 readyState 属性判定调用哪个回调函数）
- 更新页面

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[前端进阶](https://muyiy.cn/)

[SVG 参考手册](https://www.runoob.com/svg/svg-reference.html)

[SVG 图像入门教程](https://www.ruanyifeng.com/blog/2018/08/svg.html)

[掌握移动 web 开发基础系列--viewport](https://juejin.cn/post/6844903841100464136)

[移动端适配](https://jonny-wei.github.io/blog/mobile/h5/mobile-fit.html#%E5%83%8F%E7%B4%A0%E7%9B%B8%E5%85%B3%E6%A6%82%E5%BF%B5)

[Ajax - Web 开发者指南 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/Guide/AJAX)

[Ajax 原理一篇就够了](https://juejin.cn/post/6844903618764603399)
