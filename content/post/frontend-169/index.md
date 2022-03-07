---
title: '毎日のフロントエンド  169'
date: 2022-03-04T14:28:14+09:00
description: frontend 每日一练
categories:
  - HTML
  - CSS
  - JavaScript
---

## HTML

### `Data URI`

- 概念：把小文件直接嵌入文档的方案
- 格式：`data:[<mediatype>][;base64],<data>`
  - example: 一个会执行 JavaScript alert 的 HTML 文档
    - `data:text/html,<script>alert('hi');</script>`

## CSS

### `currentColor`

- `currentColor`是 color 属性的值，指：currentColor 关键字的使用值是 color 属性值的计算值。如果 currentColor 关键字被应用在 color 属性自身，则相当于是 color: inherit。

- 凡需要使用颜色值的地方，都可以使用 currentColor 替换

```html
<a href="##" class="link"><i class="icon icon1"></i>返回</a>
```

```css
.link:hover {
  color: #333;
} /* 虽然改变的是文字颜色，但是图标颜色也一起变化了 */
```

```css
.icon {
  display: inline-block;
  width: 16px;
  height: 20px;
  background-image: url(../201307/sprite_icons.png);
  background-color: currentColor; /* 该颜色控制图标的颜色 */
}
```

## JavaScript

### `ArrayBuffer`

- `ArrayBuffer` 对象用来表示通用的、固定长度的原始二进制数据缓冲区
- 它是一个字节数组，通常在其他语言中称为“byte array”。

---

- `ArrayBuffer` 不能直接操作，而是要通过类型数组对象或 DataView 对象来操作
- `Array`: 是 JavaScript 数组，可直接修改

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[Data URLs - HTTP | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Basics_of_HTTP/Data_URIs)

[`<color>` - CSS: Cascading Style Sheets | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value#currentcolor_keyword)

[currentColor-CSS3 CSS 变量](https://www.zhangxinxu.com/wordpress/2014/10/currentcolor-css3-powerful-css-keyword/)

[ArrayBuffer - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer)
