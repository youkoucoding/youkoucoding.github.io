---
title: '毎日のフロントエンド  174'
date: 2022-03-12T21:13:25+09:00
description: frontend 每日一练
categories:
  - HTML
  - CSS
  - JavaScript
---

## HTML

### `img`、`input`标签它们是行内元素还是块级元素

- 属于行内元素，也是替换元素（行内块元素）
  - 简单的说行内元素、也就是 `display:inline;` 的元素，行内块元素是 `display:inline-block` 的元素，典型的有 input
  - 行内元素，只有设置左右的 margin 和 padding，不能设置高宽，它的高度取决于内部文字的行高。宽度取决于内部文字的多少。
  - 行内块元素和块级元素的属性基本一致。可以设高宽、边距。不同在于它允许左右存在元素。而块级元素就算只有 1px 也不允许有元素和它共享一行。
  - 比如：`button`、`input`、 `textarea`、`select`、 `img`

## CSS

### css3 和 css 的区别

- CSS: Css stands for Cascading style sheet. Its main objective is to provide styling and fashion to the web page. CSS provides color, layout, background, font and border properties. CSS features allow better content accessibility, enhanced flexibility, and control, as well as the specification of the characteristics of presentation.

- CSS3: CSS3 stands for Cascading style sheet level 3, which is the advanced version of CSS. It is used for structuring, styling, and formatting web pages. Several new features have been added to CSS3 and it is supported by all modern web browsers. The most important feature of CSS3 is the splitting of CSS standards into separate modules that are simpler to learn and use.

|     | CSS                                                                                                                            | CSS3                                                                                                                                                          |
| --- | ------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | CSS is capable of positioning texts and objects. CSS is somehow backward compatible with CSS3.                                 | On the other hand, CSS3 is capable of making the web page more attractive and take less time to create it. If you write CSS3 code in CSS, it will be invalid. |
| 2   | Responsive designing is not supported in CSS                                                                                   | CSS3 is the latest version, hence it supports responsive design.                                                                                              |
| 3   | CSS cannot be split into modules.                                                                                              | Whereas, CSS3 can be breakdown into modules.                                                                                                                  |
| 4   | Using CSS, we cannot build 3D animation and transformation.                                                                    | But in CSS3 we can perform all kinds of animation and transformations as it supports animation and 3D transformations.                                        |
| 5   | CSS is very slow as compared to CSS3                                                                                           | Whereas, CSS3 is faster than CSS.                                                                                                                             |
| 6   | In CSS we have a good collection of unique color schemas and standard color.                                                   | Whereas, CSS3 has a good collection of HSL RGBA, HSLA, and gradient colors.                                                                                   |
| 7   | In CSS we can only use single text blocks.                                                                                     | But in CSS3 we can use multi-column text blocks                                                                                                               |
| 8   | CSS does not support media queries.                                                                                            | But CSS3 supports media queries                                                                                                                               |
| 9   | CSS codes are not supported by all types of modern browsers.                                                                   | Being the latest version, CSS3 codes are supported by all modern browsers.                                                                                    |
| 10  | In CSS, designers have to manually develop rounded gradients and corners.                                                      | But CSS3 provide codes for setting rounded gradients and corners                                                                                              |
| 11  | There is no special effect like shadowing text, text animation, etc. in CSS. The animation was coded in jQuery and JavaScript. | CSS3 has many advance features like text shadows, visual effects, and a wide range of font style and color.                                                   |
| 12  | In CSS, the user can add background colors to list items and lists, set images for the list items, etc.                        | Whereas, CSS3 list has a special display property defined in it. Even list items also have counter reset properties.                                          |

- New features of CSS3:
  - **Combinator**: CSS3 has a new General sibling combinator which matches up with sibling elements via the tilde (`~`) combinator.
  - **CSS Selectors**: CSS3 selectors are much advanced in comparison to simple selectors offered by CSS, and are termed as a sequence of easy to use and simple selectors.
  - **Pseudo-elements**: Plenty of new pseudo-elements have been added to CSS3 to give easy styling in depth. Even a new convention of double colons `::` is also added.
  - **Border Style**: The latest CSS3 also has new border styling features like border-radius, image-slice, image-source, and values for “width stretch”, etc.
  - **Background style properties**: New features like background-clip, size, style, and origin properties have been added to CSS3.

## JavaScript

### 断点续传的原理

- `206` `Partial Content` 状态码
- 客户端使用请求头 Range 告知自己需要的数据范围；服务器使用响应头 `Content-Range` 说明返回的数据范围和数据长度。

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[HTTP 请求范围 - HTTP | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Range_requests)

[206 Partial Content - HTTP | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/206)

[Difference between CSS and CSS3 - GeeksforGeeks](https://www.geeksforgeeks.org/difference-between-css-and-css3/)
