---
title: '毎日のフロントエンド 321~325'
date: 2022-06-04T11:57:02+09:00
description: frontend 每日一练
categories:
  - CSS
  - JavaScript
---

## CSS

### css 实现悬浮提示文本

```html
<div class="wrap">
  我是一个元素
  <div class="tips">这是悬浮提示文字</div>
</div>

<style>
  .wrap {
    position: relative;
    display: inline-block;
    margin: 4em;
  }

  .tips {
    position: absolute;
    top: -2em;
    left: 50%;
    display: none;
    white-space: nowrap;
    transform: translate(-50%, 0);
  }

  .wrap:hover .tips {
    display: block;
  }
</style>
```

---

- 伪类 `content: attr()` 获取自定义标签的内容
- [提示气泡 - CSS Tricks](https://lhammer.cn/You-need-to-know-css/#/zh-cn/poptip)

## JavaScript

### IoC

- [细数 Javascript 技术栈中的四种依赖注入](https://www.cnblogs.com/front-end-ralph/p/5208045.html)

### commonJS 模块与 ES 模块的差异

- CommonJS 模块输出的是一个值的复制，ES6 模块输出的是值引用
- CommonJS 模块是运行时加载，ES6 模块是编译时输出接口

## Reference

- [fe-interview/history.md](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

- [CSS Tricks - you need to know css](https://lhammer.cn/You-need-to-know-css/#/)
