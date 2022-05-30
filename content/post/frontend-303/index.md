---
title: '毎日のフロントエンド 303~304'
date: 2022-05-30T21:43:28+09:00
description: frontend 每日一练
categories:
  - HTML
  - Tips
---

## HTML

### html 的嵌套规范

#### HTML 标签包括 块级元素(block)、行内元素（inline）

- 块级元素，一般用来搭建网站架构、布局、承载内容……它包括以下这些标签：

```
address、blockquote、center、dir、div、dl、dt、dd、fieldset、form、h1~h6、hr、isindex、menu、noframes、noscript、ol、p、pre、table、ul
```

- 行内元素　一般用在网站内容之中的某些细节或部位，用以“强调、区分样式、上标、下标、锚点”等等，下面这些标签都属于内嵌元素：

```
a、abbr、acronym、b、bdo、big、br、cite、code、dfn、em、font、i、img、input、kbd、label、q、s、samp、select、small、span、strike、strong、sub、sup、textarea、tt、u、var
```

#### HTML 标签的嵌套规则

1. 块元素可以包含内联元素或某些块元素，但**内联元素却不能包含块元素，它只能包含其它的内联元素**
2. 块级元素不能放在`<p>`里面：
3. 有几个特殊的块级元素只能包含内嵌元素，不能再包含块级元素，这几个特殊的标签是：

```
h1、h2、h3、h4、h5、h6、p、dt
```

4. li 内可以包含 div 标签：
   - `li` 和 `div` 标 签都是装载内容的容器，没有级别之分（例如：h1、h2），li 标签连它的父级 ul 或者是 ol 都 可以容纳
5. 块级元素与块级元素并列、内嵌元素与内嵌元素并列：
6. `a`标签不能嵌套`a`

### img alt 属性

- alt 属性是一个必需的属性，它规定在图像无法显示时的替代文本
- 加 alt 属性提示文本的好处：
  - 有利于 SEO 优化，让搜索引擎爬虫蜘蛛爬取关键词
  - 在很多情况下用户无法查看图像，可以让用户明白图片的意思，提高用户体验
  - 搜索引擎会对网站的 title 以及 alt 分析，进而分类进行排序

## CSS

### img 标签是行内元素，为什么能设置宽高

- 置换元素（Replaced Element）：可以设置宽高,有自己的属性，和 inline-block 有一样的属性，主要是指 `img`、`input`、`textarea`、`select`、`object` 等这类默认有宽高样式的元素。浏览器根据元素的标签和属性，来决定元素的具体显示内容,例如：浏览器根据标签的 src 属性显示图片。根据 type 属性决定显示输入框还是按钮

## Reference

- [fe-interview/history.md](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

- [可替换元素 - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Replaced_element)
