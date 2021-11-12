---
title: '毎日のフロントエンド　57'
date: 2021-11-12T16:05:02+09:00
description: frontend 每日一练
image: frontend-57-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第五十七日

## HTML

### **Question:** `HTML5`的`download`属性

`HTMLAnchorElement.download`属性是一个`DOMString`，表明链接的资源将被下载，而不是显示在浏览器中。该值表示下载文件的建议名称。

常规的`<a>`标签通过 `href` 实现链接跳转，如果只想下载文件而不是跳转预览，最好的方式是在`<a>`标签中添加`download`属性，就能很简单地实现下载操作。

`download`是 HTML5 中`<a>`标签新增的一个属性，此属性会**强制触发下载操作**，指示浏览器下载 `URL` 而不是导航到它，并提示用户将其保存为本地文件，例如

`<a href="result.png" download>download</a>`

## CSS

### **Question:** `inline`、`block`、`inline-block`这三个属性值有什么区别

CSS 显示模块分为内部显示类型和外部显示类型，内部显示类型是定义子元素如何参与内部布局，外部显示类型定义了父元素如何参与外部整个文档流的布局:

`inline`-> `inline-inline`;
`block`-> `block-block`;
`inline-block`-> `inline-block`;

> `inline`： 行内元素，元素不独占一行，不可以修改宽高;
> `block`： 块级元素，元素独占一行，可以修改宽高;
> `inline-block`： 行内块级元素，元素不独占一行，并且可以修改宽高

## JavaScript

### **Question:** 写一个方法，使得`sum(x)(y)`和`sum(x,y)`返回的结果相同

1. `sum(x)(y)`和`sum(x,y)`返回的结果相同

```js
const sum = function (x) {
  if (argument[1]) {
    return x + argument[1];
  } else {
    return function (y) {
      return x + y;
    };
  }
};

console.log(sum(3, 4)); // 7

console.log(sum(3)(4)); // 7
```

## Reference

[haizlin/fe-interview: HTML/CSS/JavaScript/Vue/React/Nodejs/TypeScript/ECMAScritpt/Webpack/Jquery/小程序/软技能……](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)

[浅析 HTML5 中的 download 属性](https://zhuanlan.zhihu.com/p/58888918)

[JavaScript 高阶函数浅析](https://github.com/yygmind/blog/issues/36)

[Lodash Documentation # curry](https://lodash.com/docs/4.17.15#curry)
