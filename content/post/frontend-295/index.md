---
title: '毎日のフロントエンド 295'
date: 2022-05-27T23:20:28+09:00
description: frontend 每日一练
categories:
  - HTML
  - Tips
---

## HTML

### `<article>` 和 `<section>` 区别

- `<article>` 标签定义外部的内容，比如来自 blog 的文本。其内容独立于文档的其余部分
- `<section>` 标签定义文档中的节（section、区段）。比如章节、页眉、页脚或文档中的其他部分

## CSS

### `body{height:100%}`和`html,body{height:100%}`有什么区别？为什么 html 要设置`height:100%`

- html 是 body 的父级，在缺少了父级的宽高之后，如果给 body 设置一个渐变色背景的话将不会正常显示
- 一个 div 块级元素没有主动为其设置宽度和高度，浏览器会为其分配可使用的最大宽度(比如全屏宽度)，但是不负责分配高度，块级元素的高度是由子元素堆砌撑起来的。那么，html 和 body 标签的高度也都是由子级元素堆砌撑起来的
- 元素高度百分比需要向上遍历父标签要找到一个定值高度才能起作用，如果中途有个 height 为 auto 或是没有设置 height 属性，则高度百分比不起作用，此时的情况是父元素高度依赖子元素堆砌撑高，而子元素依赖父元素的定高起作用，互相依赖，却都无法依赖(body 如果为默认设置，它的高度值为 auto，会根据子元素的高度来支撑高度。倘若子元素的高度设置为依赖父元素(body)的高度来支持的百分比数值，那么就形成了悖论。最后浏览器找不到计算高度的情况下，body 的高度实际被设置为 0)

```css
html,
body {
  height: 100%;
  margin: 0;
  padding: 0;
}
```

## Tips

## Reference

- [fe-interview/history.md](https://github.com/haizlin/fe-interview/blob/master/category/history.md)
