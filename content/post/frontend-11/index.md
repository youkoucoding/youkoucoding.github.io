---
title: '毎日のフロントエンド　11'
date: 2021-09-25T23:02:39+09:00
description: frontend 每日一练
image: frontend-11-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第十一日

## HTML

**#Question:** 你对标签语义化的理解是什么？

### 什么是 HTML 语义化标签

语义化的标签，旨在让标签有自己的含义。

```html
<p>一行文字</p>
<span>一行文字</span>
```

如上代码，`p` 标签与 `span` 标签都区别之一就是，`p` 标签的含义是：段落。而 `span` 标签责没有独特的含义。

### 语义化标签的优势

- 语义化更具有可读性，便于团队开发维护
- 在没有 css 的情况下，页面也可以呈现出也很好的内容结构和代码结构
- `SEO`，搜索引擎能更好的理解页面中各部分的关系，可更快更准确的搜索到信息

### 常见的语义化标签

> 因此我们在写页面结构时，应尽量使用有 语义的 HTML 标签

- `<title>`：页面主体内容。
- `<hn>`：h1~h6，分级标题，<h1> 与 <title> 协调有利于搜索引擎优化。
- `<ul>`：无序列表。
- `<li>`：有序列表。
- `<header>`：页眉通常包括网站标志、主导航、全站链接以及搜索框。
- `<nav>`：标记导航，仅对文档中重要的链接群使用。
- `<main>`：页面主要内容，一个页面只能使用一次。如果是 web 应用，则包围其主要功能。
- `<article>`：定义外部的内容，其中的内容独立于文档的其余部分。
- `<section>`：定义文档中的节（section、区段）。比如章节、页眉、页脚或文档中的其他部分。
- `<aside>`：定义其所处内容之外的内容。如侧栏、文章的一组链接、广告、友情链接、相关产品列表等。
- `<footer>`：页脚，只有当父级是 body 时，才是整个页面的页脚。
- `<small>`：呈现小号字体效果，指定细则，输入免责声明、注解、署名、版权。
- `<strong>`：和 em 标签一样，用于强调文本，但它强调的程度更强一些。
- `<em>`：将其中的文本表示为强调的内容，表现为斜体。
- `<mark>`：使用黄色突出显示部分文本。
- `<figure>`：规定独立的流内容（图像、图表、照片、代码等等）（默认有 40px 左右 margin）。
- `<figcaption>`：定义 figure 元素的标题，应该被置于 figure 元素的第一个或最后一个子元素的位置。
- `<cite>`：表示所包含的文本对某个参考文献的引用，比如书籍或者杂志的标题。
- `<blockquoto>`：定义块引用，块引用拥有它们自己的空间。
- `<q>`：短的引述（跨浏览器问题，尽量避免使用）。
- `<time>`：datetime 属性遵循特定格式，如果忽略此属性，文本内容必须是合法的日期或者时间格式。
- `<abbr>`：简称或缩写。
- `<dfn>`：定义术语元素，与定义必须紧挨着，可以在描述列表 dl 元素中使用。
- `<address>`：作者、相关人士或组织的联系信息（电子邮件地址、指向联系信息页的链接）。
- `<del>`：移除的内容。
- `<ins>`：添加的内容。
- `<code>`：标记代码。
- `<meter>`：定义已知范围或分数值内的标量测量。（Internet Explorer 不支持 meter 标签）
- `<progress>`：定义运行中的进度（进程）。

## CSS

**#Question:** css 常用的布局方式有哪些？

- 流式布局: 最基本的布局，就是顺着 html 像流水一样流下来
- 绝对定位: 利用 position: absolute 进行绝对定位的布局
- `float` 布局: 最初用来解决多栏布局的问题。比如 圣杯、双飞翼 的布局都可以用 float 来实现
- 珊格布局: bootstrap 用的布局，把页面分为 24 分，通过 row 和 col 进行布局
- `flex` 布局: css3 的布局可以非常灵活地进行布局和排版
- `grid` 布局: 网格布局

## JavaScript

**#Question:** 简要描述下 JS 有哪些内置的对象 `built-in objects`

[JavaScript 标准内置对象 - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects)

[JS 所有内置对象属性和方法汇总 - SegmentFault 思否](https://segmentfault.com/a/1190000011467723)

## Reference

[JS 所有内置对象属性和方法汇总](https://segmentfault.com/a/1190000011467723)

[JavaScript 标准内置对象 - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects)

[前端面试每日 3+1-以前端面试题来驱动学习，提倡每日学习与思考，每天进步一点！](http://www.h-camel.com/index.html)
