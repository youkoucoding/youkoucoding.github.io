---
title: '毎日のフロントエンド  117'
date: 2022-01-11T12:05:24+09:00
description: frontend 每日一练
image: frontend-117-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第一百一十七日

## HTML

### **Question:** frame 和 iframe 有什么区别

> `frame` 在 HTML5 中已经被废弃。现在比较推荐使用 iframe 来代替 frame

- 两者都可以在一个 HTML 中插入引入另一个 HTML 的文档内容。frame 必须要放在 frameset 中使用，可以想象把浏览器的窗口进行切割
- `frame` 不能脱离 frameset 使用，并且不能放在 body 中，否则不能正常显示，而 iframe 则没有这个限制

## CSS

### **Question:** CSS 中的字母"C"代表什么

CSS(Cascading Style Sheets)。"C" 即为 `Cascading` 层叠的意思，编写 CSS 的时候，写在后面的样式会覆盖前面的样式即层叠。

## JavaScript

### **Question:** `document.write`和`innerHTML`有什么区别

- `document.write()` **是 DOM 方法** 将需要展示的内容添加到 HTML 文档流中。对于一个已经加载完成的页面，document.write() 会**重新绘制整个页面**。自然其性能就不是很好。
  - 直接写入到页面的内容流，如果在写之前没有调用 document.open, 浏览器会自动调用 open。每次写完关闭之后重新调用该函数，会导致页面被重写。
- `Element.innerHTML` **是 DOM 属性**, 替换某个元素中的内容，简单地认为是 `<div></div>` 标签中间的内容。**也即只会影响到所指定的元素**。

- 两者都可向页面输出内容,`innerHTML`比`document.write()`更灵活

  - 当文档加载时调用 document.write 直接向页面输出内容，文档加载结束后调用 document.write 输出内容会重写整个页面。通常按照两种的方式使用 write() 方法：一是在使用该方在文档中输出 HTML，二是在调用该方法的的窗口之外的窗口、框架中产生新文档（务必使用 close 关闭文档）。
  - 在读模式下，innerHTML 属性返回与调用元素的所有子节点对应的 HTML 标记，在写模式下，innerHTML 会根据指定的值创建新的 DOM 树替换调用元素原先的所有子节点。

- 两者都可动态包含外部资源如 JavaScript 文件
  - `document.write`插入`<script>`元素会自动执行其中的脚本
  - 大多数浏览器中，通过`innerHTML`插入`<script>`元素并不会执行其中的脚本。

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[前端进阶](https://muyiy.cn/)

[`<iframe>` - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe)

[document.write - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/write)

[element.innerHTML - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/innerHTML)
