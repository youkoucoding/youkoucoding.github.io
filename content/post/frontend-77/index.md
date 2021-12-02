---
title: '毎日のフロントエンド　 77'
date: 2021-12-02T20:41:22+09:00
description: frontend 每日一练
image: frontend-77-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第七十七日

## HTML

### **Question:** `HTML5`的`img`标签属性 `srcset` 和 `sizes` 的理解？都有哪些应用场景？

`HTMLImageElement.srcset`, `srcset` 的值是一个字符串，用来定义一个或多个图像候选地址，以 `,`分割，每个候选地址将在特定条件下得以使用。候选地址包含图片 `URL` 和一个可选的宽度描述符和像素密度描述符，该候选地址用来在特定条件下替代原始地址成为 `src (en-US)` 的属性。

> The `srcset` property, **along with the `sizes` (en-US) property**, are a crucial component in **designing responsive web sites**, as they can be used together to make pages that use appropriate images for the rendering situation.

- `srcset`：规定了图片的 `src` 候选集;
- `sizes`：规定了图片在不同条件下的尺寸取值，根据尺寸取值从`srcset`中找到对应的图片`src`；配合`srcset`属性才能使用；

多用于响应式图片或不同像素密度设备的图片适配

## CSS

### **Question:** 请问`display:inline-block`在什么时候会显示间隙

- `display: inline-block` 在`IE67`下会产生`4px`的空白间隙，原因是换行或空格会占据一定的位置。

- 解决办法： 设置父元素的`font-size: 0; letter-spaceing: -4px;`

## JavaScript

### **Question:** 解释：`var x, y = 1; x + y = ?`

```js
x; // => undefined
y; // => 1
x + y; // => undefined + 1 => NaN
```

运算符 `+` 的`implicit type conversion`规则：

- 若任何一侧是 `string` 则两边转换为 `string` 进行连接
- `object` 则依次调用`valueOf`和`toString`
- 否则均转换为 `number` 并进行相加
- `symbol` 相加, 会 `throw TypeError`

### **Question:** 写出执行结果,并解释原因

```js
const value = 'Value is' + !!Number(['0']) ? 'yideng' : 'undefined';
console.log(value);
```

Result: yideng

- `+`优先级大于`?`
- 等价于 `'Value is false' ? 'yideng' : 'undefined'` 而不是 `'Value is' + (false ? 'yideng' : 'undefined')`

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)

[HTMLImageElement.srcset - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLImageElement/srcset)

[响应式图片 srcset 全新释义 sizes 属性 w 描述符](https://www.zhangxinxu.com/wordpress/2014/10/responsive-images-srcset-size-w-descriptor/)
