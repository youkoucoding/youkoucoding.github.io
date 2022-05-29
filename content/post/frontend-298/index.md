---
title: '毎日のフロントエンド 298'
date: 2022-05-29T14:57:59+09:00
description: frontend 每日一练
categories:
  - HTML
  - Tips
---

## CSS

### `initial`、`inherit`、`unset`、`revert`

- initial（初始）、inherit（继承）、unset（未设置）、revert（还原）

- `inherit` 可以继承父级元素的属性
- `initial`则是不继承
- `unset` 表示如果该属性默认可继承，则值为`inherit`，否则值为`initial`
- `revert` 表示样式表中定义的元素属性的默认值。若用户定义样式表中显式设置，则按此设置；否则，按照浏览器定义样式表中的样式设置；否则，等价于`unset`

### 取消从父级元素继承下来的 CSS 样式

- 如果是恢复单个属性样式，例如 font-size，可以使用
  - `font-size: initial;`
- 如果是将所有属性样式恢复为默认状态，可以使用
  - `all: intial;`

### `height:100%`和`height:inherit`

- 父容器`height: auto`，无论`height:100%`或者`height:inherit`表现都是`auto`
- 父容器定高`height: 100px`，无论`height:100%`或者`height:inherit`表现都是`100px`

---

- 当子元素为**绝对定位元素**，同时，父容器的`position: static`的时候

  - [height:100%和 height:inherit 差异演示](https://www.zhangxinxu.com/study/201502/height-100-height-inherit.html)

  - `height:inherit` 高度自适应没有定位特性的父级元素, 容器高度变化了，里面的绝对定位元素依然高度自适应

## JavaScript

### 删除字符串中所有相邻重复的项

- `'aabbaaaaccdeee'.replace(/(.)\1*/g, '$1'); // abacde`

## Tips

### `304`

- 协商缓存: 向服务器发送请求，服务器会根据这个请求的 request header 的一些参数来判断是否命中协商缓存，如果命中，则返回 304 状态码并带上新的 response header 通知浏览器从缓存中读取资源；
- 请求头：`if-modified-since` `if-none-match` 与 响应头：`last-modified` `etag` 比较

## Reference

- [fe-interview/history.md](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

- [HTML `<area><map>`标签及在实际开发中的应用](https://www.zhangxinxu.com/wordpress/2017/05/html-area-map/)

- [CSS 中 height:100%和 height:inherit 的异同](https://www.zhangxinxu.com/wordpress/2015/02/different-height-100-height-inherit/)
