---
title: '毎日のフロントエンド 311~317'
date: 2022-06-02T21:08:49+09:00
description: frontend 每日一练
categories:
  - CSS
  - JavaScript
---

## HTML

### 禁止 web 端的页面缩放

- `<meta name="viewport" content="width=device-width, initial-scale=1,user-scalable=0">`

## CSS

### 3 像素 Bug

> img 标签渲染之后下方会出现几个像素（我用谷歌测试是 4px, 火狐 3.5px）的空白；

- img 是行内元素，默认 display：inline; 它与文本的默认行为类似，下边缘是与基线对齐，而不是贴紧容器下边缘，所以会有几像素的空白；
- 解决办法：
  1. img 设置为`display: block;`
  2. img 和其父元素都设置`vertical-align: top;` 让其 top 对齐而不是 baseline 对齐；
  3. img 父元素设置`line-height: 0; font-size: 0`
  4. img 标签 `display:block; vertical-align :middle`

### 自动切换页面的颜色

- `prefers-color-scheme`
- [prefers-color-scheme - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@media/prefers-color-scheme)

### 能用子代选择器的时候不要用后代选择器

- 选择从右到左依次解析匹配，所以后代选择器会去找它的所有父级
- 子代选择器只会选择**直接的父级**；减少匹配次数，提高效率

## Reference

- [fe-interview/history.md](https://github.com/haizlin/fe-interview/blob/master/category/history.md)
