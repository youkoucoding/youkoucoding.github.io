---
title: '毎日のフロントエンド  135'
date: 2022-01-29T13:24:15+09:00
description: frontend 每日一练
image: frontend-135-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第一百三十五日

## HTML

### **Question:** 如何刷新浏览器的应用缓存

浏览器缓存可分为**强缓存**和**协商缓存**

- **强缓存**指的是设置了 `expires` 或者 `cache-control:max-age` 的请求，此类`NaN`本身设定的过期时间之前刷新不会再次向浏览器发起请求，直接由客户端决定使用缓存进行页面展示。

- **协商缓存**指的是未设置强缓存对应字段的缓存，浏览器会向服务器发送请求，并带`IF-Modified-Since`和`IF-None-Match`字段，服务器对应的返回字段为`Last-Modified`或 `Etag`，如果在 etag 未更改 或 last-modified 的时间早于 IF-Modified-Since 则服务器不返回文件，使用浏览器本地缓存。

- 如何刷新应用缓存

  - 点击浏览器的刷新按钮或者 F5 刷新时，浏览器会**忽略强缓存**, 必定向服务器发起请求，但是如果服务器返回 304 则会继续使用本地缓存。
  - 点击**Ctrl+F5** 浏览器会**忽略一切缓存**（`cache-control:no-cache`），向服务器发起请求，并且一定会使用服务器的返回来渲染页面。

## CSS

### **Question:** `will-change`属性

告诉浏览器,这个元素的某些属性可能会频繁变动触发回流，要求浏览器给予资源进行优化，一般浏览器会给这个元素单独生成一个图层渲染,gpu 加速等提前优化手段，不应过度使用这个属性,这属性只是性能出现问题的最后手段

## JavaScript

### **Question:** 解释下 NaN === NaN 的结果

```js
NaN === NaN;
// false
```

- `NaN`不等于任何值，包括`NaN`本身

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[will-change - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/will-change)
