---
title: '毎日のフロントエンド　25'
date: 2021-10-11T16:07:37+09:00
description: frontend 每日一练
image: frontend-25-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第二十五日

## HTML

### **#Question:** 请说说`<script>`、`<script async>`和`<script defer>`的区别

![script  deference](script-explanation.png)

- `<script>` : 加载的时候是同步的会阻塞后面代码的执行，加载立即执行
- `<script async>`: 异步加载，加载和执行是并行的
- `<script defer>`: 异步加载，需等到所有文档加载完才执行
- `async` `defer`这两个属性无法应用于内联`script`

The modern websites, scripts are often "heavier" than HTML: their download size is larger, anb processing time is also longer.

- Scripts with `defer` never block the page
- Scripts with `defer` always execute when the DOM is ready(but before `DOMContentLoaded` event).

The `async` attribute means that a script is completely independent:

- The browser doesn't block on `async` script like `defer`
- Other scripts don't wait for `async` scripts, and `async` scripts don't wait for them.
- `DOMContentLoaded` and async scripts don't wait for each other:

  - `DOMContentLoaded` may happen both before an async script (if an async script finishes loading after the page is complete)
  - ... or after an async script(if an async script is short or was in HTTP-cache)

In other words, `async` scripts load in the background and run when ready. The DOM and other scripts don't wait for them, and they don't wait for anything. A fully independent script that runs when loaded.

#### Summary

Both `async` and `defer` have one commmon thing: **donwloading of such scripts don't block page rendering.**

So the user can read page content and get acquainted with the page immediately.

In practice:

- `defer` is used for scripts that need the whole DOM and/or their relative execution order is important.
- `Async` is used for independent scripts, like **counters or ads.** And their relative execution order does not matter.

## CSS

### **#Question:** 在页面中的应该使用奇数还是偶数的字体？为什么呢？

1. 尽量使用偶数字号
2. 偶数字号容易和页面其他标签的其他属性形成比例关系

## JavaScript

### **#Question:** 写一个判断设备来源的方法

> [current-device: The easiest way to write conditional CSS and/or JavaScript based on device operating system](https://github.com/matthewhudson/current-device)

`navigator.userAgent`

## Reference

[Scripts: async, defer](https://javascript.info/script-async-defer)

[前端面试每日 3+1-以前端面试题来驱动学习，提倡每日学习与思考，每天进步一点！](http://www.h-camel.com/index.html)
