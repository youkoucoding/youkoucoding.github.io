---
title: '毎日のフロントエンド 191'
date: 2022-03-29T17:36:42+09:00
description: frontend 每日一练
categories:
  - CSS
  - JavaScript
---

## CSS

### 举例 css 有哪些简写的属性和属性值

```css
border: solid 1px red;

border-style: solid;
border-width: 1px;
border-color: red;
```

```css
animation: fadeIn 0.5s forward ease-in 0.2s infinite;

animation-name: fadeIn;
animation-duration: 0.5s;
animation-fill-mode: forward;
animation-timing-function: ease-in;
animation-delay: 0.2s;
animation-iteration-count: infinite;
```

```css
background: url(images/foo.png) center top/cover no-repeat;

background-image: url(images/foo.png);
background-position-x: center;
background-position-y: top;
background-size: cover;
background-repeat: no-repeat;
```

```css
flex: 1;

flex: 1 1 0;

flex-grow: 1;
flex-shrink: 1;
flex-basis: 0;
```

## JavaScript

### `atob`和`btoa`

- `btoa`：binary to ascii；(base64 的**编码**)
- `atob`：ascii to binary;（base64 的**解码**）

> 无法用于 Unicode 字符

```js
// Define the string
var string = 'Hello World!';

// Encode the String
var encodedString = btoa(string);
console.log(encodedString); // Outputs: "SGVsbG8gV29ybGQh"

// Decode the String
var decodedString = atob(encodedString);
console.log(decodedString); // Outputs: "Hello World!"
```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[WindowOrWorkerGlobalScope.btoa() - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/btoa)

[WindowOrWorkerGlobalScope.atob() - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/atob)
