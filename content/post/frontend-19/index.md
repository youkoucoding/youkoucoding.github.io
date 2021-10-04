---
title: '毎日のフロントエンド　19'
date: 2021-10-04T15:37:26+09:00
description: frontend 每日一练
image: frontend-19-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第十九日

## HTML

### **#Question:** 说说你对`HTML`中的置换元素和非置换元素的理解

#### 置换元素（`Replaced Element`）

> 简单来说，置换元素可以设置宽 高,他们有自己的属性，和 `inline-block` 有一样的属性

1. 主要是指 `img`、`input`、`textarea`、`select`、`object` 等这类默认就有 `CSS` 格式化外表范围的元素。

2. 浏览器根据元素的标签和属性，来决定元素的具体显示内容

   - 如：浏览器根据标签的 `src` 属性显示图片。根据 `type` 属性决定显示输入框还是按钮

#### 非置换元素（`non-Replaced Element`）

1. 就是除了 img、input、textarea、select、object 等置换元素以外的元素

2. 内容直接展示给浏览器。例如标签，标签里的内容会被浏览器直接显示给用户

## CSS

### **#Question:** `CSS`的属性`content`有什么作用呢？有哪些场景可以用到

#### `:before` 和 `:after`

- 默认 `display: inline;`
- 必须设置 `content` 属性，否则一切都是无用功， content 属性也只能应用在 `:before` 和 `:after` **伪元素**上
- 默认 `user-select: none;`，就是 `:before` 和 `:after` 的内容无法被用户选中
- 伪元素可以和伪类结合使用形如：`.target:hover:after`
- :before 和 :after 是在 CSS2 中提出来的，所以兼容 IE8
- `::before` 和 `::after` 是 CSS3 中的写法，为了将**伪类**和伪元素区分开
- 不可通过 DOM 使用，它只是纯粹的表象。在特殊情况下，从一个访问的角度来看，当前屏幕阅读不支持生成的内容

#### `content` 定义用法

`content` 属性与 `:before` 及 `:after` **伪元素**配合使用，在元素头或尾部来插入生成内容

```css
content: normal                                /* Keywords that cannot be combined with other values */
content: none

content: 'prefix'                              /* <string> value, non-latin characters must be encoded e.g. \00A0 for &nbsp; */
content: url(http://www.example.com/test.html) /* <uri> value */
content: chapter_counter                       /* <counter> values */
content: attr(value string)                    /* attr() value linked to the HTML attribute value */
content: open-quote                            /* Language- and position-dependant keywords */
content: close-quote
content: no-open-quote
content: no-close-quote

content: open-quote chapter_counter            /* Except for normal and none, several values can be used simultaneously */

content: inherit

```

## JavaScript

### **#Question:** `attribute` 和 `property` 有什么不同

- `property`是 `DOM` 中的属性，是 JavaScript 里的对象

  - JavaScript の世界の住人
  - 扱いやすいようにパースされている
  - 動的（なこともある）
  - `element.property` 例：`p.className`
  - `$().prop`
  - 可以读取标签自带属性，包括没有写出来的
  - 不能读取 `attribute` 设置的属性
  - 是元素（对象）的属性

- `attribute` 是 `HTML` 标签上的特性，它的值**只能是字符串**,直接在 html 标签添加的都是`attribute`属性

  - `HTML` の世界の住人
  - `HTML` に書いたものがそのまま出る
  - 静的
  - `get/setAttribute()` 例如：`a.getAttribute('href')`
  - `$().attr`
  - 不能读取 `property` 设置的属性

## Reference

[前端面试每日 3+1-以前端面试题来驱动学习，提倡每日学习与思考，每天进步一点！](http://www.h-camel.com/index.html)

[Attribute と Property - Qiita](https://qiita.com/jkr_2255/items/66a16bd969454ee8b114)
