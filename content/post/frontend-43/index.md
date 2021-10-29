---
title: '毎日のフロントエンド　43'
date: 2021-10-29T10:43:39+09:00
description: frontend 每日一练
image: frontend-43-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第四十三日

## HTML

### **Question:** 如何让元素固定在页面底部？有哪些比较好的实践

```html
<div class="wrapper">
  <div class="content"><!-- 页面主体内容区域 --></div>
  <div class="footer"><!-- 需要做到 Sticky Footer 效果的页脚 --></div>
</div>
```

方案一：`absolute`

```css
/* content 的 padding-bottom 需要与 footer 的 height 一致 */
html,
body {
  height: 100%;
}
.wrapper {
  position: relative;
  min-height: 100%;
  padding-bottom: 50px;
  box-sizing: border-box;
}
.footer {
  position: absolute;
  bottom: 0;
  height: 50px;
}
```

方案二：`calc`

```css
.content {
  min-height: calc(100vh - 50px);
}
.footer {
  height: 50px;
}
```

方案三：`table`

```css
html,
body {
  height: 100%;
}
.wrapper {
  display: table;
  width: 100%;
  min-height: 100%;
}
.content {
  display: table-row;
  height: 100%;
}
```

**方案四：`Flexbox`**

```css
html {
  height: 100%;
}
body {
  min-height: 100%;
  display: flex;
  flex-direction: column;
}
.content {
  flex: 1;
}
```

方案五：`grid`

```css
html {
  height: 100%;
}
body {
  min-height: 100%;
  display: grid;
  grid-template-rows: 1fr auto;
}
.footer {
  grid-row-start: 2;
  grid-row-end: 3;
}
```

## CSS

### **Question:** span 与 span 之间有看不见的空白间隔是什么原因引起的？有什么解决办法

_产生空白的原因：_ 元素被当成行内元素排版的时候，元素之间的空白符（空格、回车换行等）都会被浏览器转换成一个空白字符，这个字符的大小受`font-size`影响

solution:

- 去掉换行，将 `span` 写成一行 `<span>hello</span><span>world</span>`
- **父元素**使用 `flex` 布局：`.wrap {display: flex; flex-direction: row;}`
- **父元素**设置 `font-size: 0;` `span` 子元素再设置字体大小 `font-size: 16px;`
- `span` **子元素**设置 `float: left`

## JavaScript

### **Question:** 能不能简单概括一下`jQuery` 的实现原理

```js
(function (window, undefined, document) {
  function jQuery(prop) {
    return new jQuery.prototype.init();
  }
  jQuery.prototype = {
    contructor: jQuery,
    init: function (prop) {},
    //  ...
  };
  jQuery.prototype.init.prototype = jQuery.prototype;
  window['jQuery'] = window['$'] = new jQuery();
})(window, undefined, document);
```

`jQuery` 是通过封装浏览器原生的 `DOM API` 实现 `dom` 元素的选取，然后封装到 `jQuery` 对象中去，同时根据浏览器检测对不同浏览器操作不同的 `APi` .`jQuery` 对象上高度集成了`API`。当然 jQuery 还有做的更多比如，可以 `new jQuery('div')`,也可以直接`$('div')`,这个巧妙地运算就是上面`init`方法；如果页面已经有`$`时，`jQuery` 会先将`$`接管把之前$的全局名保存下来

## Reference

[haizlin/fe-interview: HTML/CSS/JavaScript/Vue/React/Nodejs/TypeScript/ECMAScritpt/Webpack/Jquery/小程序/软技能……](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)

[JELLY | Sticky Footer，完美的绝对底部](https://jelly.jd.com/article/6006b1045b6c6a01506c87e3)

[Sticky Footer, Five Ways | CSS-Tricks](https://css-tricks.com/couple-takes-sticky-footer/)
