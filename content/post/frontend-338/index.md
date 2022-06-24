---
title: '毎日のフロントエンド 338'
date: 2022-06-23T23:41:39+09:00
draft: true
description: frontend 每日一练
categories:
  - HTML
  - CSS
---

## HTML

### 如何禁止 input 输入的历史记录

- form: `autocomplete=off` 可以禁止整个表单的历史记录
- input: `autocomplete=off` 可禁止这个 input 的历史记录

## CSS

### html 元素继承`box-sizing`

```css
html {
  box-sizing: border-box;
}
*,
*:before,
*:after {
  box-sizing: inherit;
}
```

## Reference

- [fe-interview/history.md](https://github.com/haizlin/fe-interview/blob/master/category/history.md)
