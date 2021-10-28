---
title: '毎日のフロントエンド　42'
date: 2021-10-28T11:47:15+09:00
description: frontend 每日一练
image: frontend-42-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第四十二日

## HTML

### **Question:** 解释下什么是`ISISO8859-2`字符集

`ISO/IEC 8859-1`，又称 Latin-1 或“西欧语言”，`ISO/IEC 8859-2 Latin-2`或“中欧语言”，是国际标准化组织内 ISO/IEC 8859 的 8 位字符集。它以 ASCII 为基础，在空置的 0xA0-0xFF 的范围内，加入 192 个字母及符号，藉以供使用变音符号的拉丁字母语言使用。

## CSS

### **Question:** 重置（初始化）css 的作用是什么

统一各个浏览器自带的默认样式, 保证各个浏览器尽量统一样式

## JavaScript

### **Question:** window 对象和 document 对象有什么区别

#### `window`对象

代表浏览器中的一个打开的窗口或者框架，`window`对象会在或者每次出现时被自动创建，在客户端`JavaScript`中，`Window`对象是全局对象`global`(`node`)，所有的表达式都在当前的环境中计算，要引用当前的窗口不需要特殊的语法，可以把那个窗口属性作为全局变量使用，例如：可以只写`document`，而不必写`window.document`。同样可以把窗口的对象方法当做函数来使用，如：只写 alert()，而不必写 window.alert.
**`window`对象实现了核心 JavaScript 所定义的全局属性和方法。**

#### `document`对象

**代表整个`HTML`文档，可以用来访问页面中的所有元素。**
每一个载入浏览器的 `HTML` 文档都会成为 `document` 对象。`document` 对象使我们可以使用脚本(js)中对 HTML 页面中的所有元素进行访问。`document` 对象是 `window` 对象的一部分可以通过 `window.document` 属性对其进行访问
`HTMLDocument` 接口进行了扩展，定义 `HTML` 专用的属性和方法，很多属性和方法都是 `HTMLCollection` 对象，其中保存了对锚、表单、链接以及其他可脚本元素的引用。

- `document`是文档对象，以`html`形式展示。是`window`对象里面的 一部分

## Reference

[haizlin/fe-interview: HTML/CSS/JavaScript/Vue/React/Nodejs/TypeScript/ECMAScritpt/Webpack/Jquery/小程序/软技能……](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)
