---
title: '毎日のフロントエンド  173'
date: 2022-03-11T15:40:11+09:00
description: frontend 每日一练
categories:
  - HTML
  - CSS
  - Tips
---

## HTML

### HTML 的标签区分大小写

- DOCTYPE 声明, 不区分大小写
  - `<!DOCTYPE html>`
  - `<!doctype html>`
  - `<!DOCTYPE HTML>`
  - `<!Doctype html>`
- HTML 文档中的标签名和属性名都是大小写**不敏感**的
- HTML 中属性的名字是大小写不敏感的，而**属性的值则默认是大小写敏感的**
  - 下述列表中的属性的值是大小写不敏感的:
    - `accept` `accept-charset` `align` `alink` `axis` `bgcolor` `charset` `checked` `clear` `codetype` `color` `compact` `declare` `defer` `dir`
    - `direction` `disabled` `enctype` `face` `frame` `hreflang` `http-equiv` `lang` `language` `link` `media` `method` `multiple` `nohref`
    - `noresize` `noshade` `nowrap` `readonly` `rel` `rev` `rules` `scope` `scrolling` `selected` `shape` `target` `text` `type (视情况而定)` `valign` `valuetype` `vlink`
- XML 文档中的标签名和属性名都是大小写敏感的。

- **大小写不敏感**
  - meta 标签中的 name
  - meta 标签中的 http-equiv
  - http-equiv="X-UA-Compatible"
  - `<meta charset="utf-8">` 或者 `<meta http-equiv="content-type" content="text/html; charset=utf-8">`

## CSS

### 字体图标

- [Font Awesome](https://fontawesome.com/)

- 矢量（无限缩放、高清）
- CSS 样式（通用、灵活）
- 体积更小（快速）

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[图标字体(IconFont)制作](https://segmentfault.com/a/1190000005614532)

[字体图标(font-icon)，你还有什么理由不使用它？](https://segmentfault.com/a/1190000006944194)
