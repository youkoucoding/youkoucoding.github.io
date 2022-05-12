---
title: '毎日のフロントエンド 243~249'
date: 2022-05-11T20:49:25+09:00
description: frontend 每日一练
categories:
  - Tips
---

## HTML

### 如何不让网页嵌入`iframe`

- HTTP 响应头信息中的`X-Frame-Options`，可以指示浏览器是否应该加载一个 iframe 中的页面。如果服务器响应头信息中没有 X-Frame-Options，则该网站存在 ClickJacking 攻击风险。网站可以通过设置 X-Frame-Options 阻止站点内的页面被其他页面嵌入从而防止点击劫持
  1. deny：表示该页面不允许在 frame 中展示，即便是在相同域名的页面中嵌套也不允许
  2. sameorigin：表示该页面可以在相同域名页面的 frame 中展示
  3. allow-from uri：表示该页面可以在指定来源的 frame 中展示

## CSS

### 代替 html5 中不再支持 table 的 `cellspacing` 和 `cellpadding` 属性

- [`border-spacing`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border-spacing)

### `em`

#### `em`

- em 是相对元素本身的 font-size 的相对单位，比如元素本身的 font-size 是 14px，那么 1.2em = 1.2 \* 14px = 16.8px。注意，是**相对元素本身**的 font-size，会随着元素的 font-size 的改变而改变

- em 用于设置元素的 padding, margin, border-radius

### `useState`

## Reference

- [fe-interview/history.md](https://github.com/haizlin/fe-interview/blob/master/category/history.md)
