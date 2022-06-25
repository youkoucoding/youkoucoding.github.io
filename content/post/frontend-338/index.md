---
title: '毎日のフロントエンド 338~339'
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

### input 上传文件可以同时选择多个

- `<input type="file" multiple />`

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

### 异步加载 CSS

- `rel="preload"`
  - `<link rel="preload" href="cssfile.css" as="style" onload="this.rel='stylesheet'">`
  - 通过 preload 属性值就是告诉浏览器这个资源文件随后会用到，请提前加载好

## Reference

- [fe-interview/history.md](https://github.com/haizlin/fe-interview/blob/master/category/history.md)
