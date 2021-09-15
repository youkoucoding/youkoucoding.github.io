---
title: '毎日のフロントエンド　2'
date: 2021-09-15T19:48:33+09:00
description: frontend 每日一练
image: frontend-1-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第二天

## HTML

**#Question:** `HTML` 的元素有哪些（包含 `HTML5`）？

块级元素 Block

> `Block` elements are meant to **structure** the main parts of your page, by dividing your content in coherent blocks.

- 常用：div、p、ul、li、ol
- 定义页面结构（Structure elements）: aside、footer、header、nav、section、main
- 文章：address、article、figure、figcaption、h1、h2、h3、h4、h5、h6、pre
- 表格：table、thead、tbody、tfoot、th、td、caption
- 表单：form
- 其他：canvas

行内元素 Inline

> `Inline` elements are meant to differentiate part of a text, to give it a oarticular function or meaning. Inline elements usually comprise a single or few words.

- 常用：a、img、span
- 文本：em、i、strong、small
- 表单：button、input、label、option、progress、select、textarea
- 媒体：audio、video

> `Block` elements can contain either block or inline elements. However, `inline` elements can only contain other inline elements.

HTML5 新增

- audio：音频
- video：视频
- header：头部
- section：内容块
- footer：底部
- aslide：侧边
- nav：导航
- address：地址

## CSS

边框(borders):

- border-radius 圆角
- box-shadow 盒阴影
- border-image 边框图像

背景:

- background-size 背景图片的尺寸
- background_origin 背景图片的定位区域
- background-clip 背景图片的绘制区域

渐变：

- linear-gradient 线性渐变
- radial-gradient 径向渐变

文本效果;

- word-break
- word-wrap
- text-overflow
- text-shadow
- text-wrap
- text-outline
- text-justify

转换：

- 2D 转换属性
- transform
- transform-origin

2D 转换方法

- translate(x,y)
- translateX(n)
- translateY(n)
- rotate(angle)
- scale(n)
- scaleX(n)
- scaleY(n)
- rotate(angle)
- matrix(n,n,n,n,n,n)

3D 转换：
\*3D 转换属性：

- transform
- transform-origin
- transform-style

3D 转换方法

- translate3d(x,y,z)
- translateX(x)
- translateY(y)
- translateZ(z)
- scale3d(x,y,z)
- scaleX(x)
- scaleY(y)
- scaleZ(z)
- rotate3d(x,y,z,angle)
- rotateX(x)
- rotateY(y)
- rotateZ(z)
- perspective(n)

过渡

- transition

动画

- @Keyframes 规则
- animation

弹性盒子(flexbox)
多媒体查询@media

**#Question:** `CSS3` 有哪些新增的特性？

## Javascript

**#Question:** 写一个方法去掉字符串中的空格

```js
const str = '  s t  r  ';

// A frozen object can no longer be changed
const POSITION = Object.freeze({
  left: Symbol(),
  right: Symbol(),
  both: Symbol(),
  center: Symbol(),
  all: Symbol(),
});

function trim(str, position = POSITION.both) {
  if (!!POSITION[position]) throw new Error('unexpected position value');

  switch (position) {
    case POSITION.left:
      str = str.replace(/^\s+/, '');
      break;
    case POSITION.right:
      str = str.replace(/\s+$/, '');
      break;
    case POSITION.both:
      str = str.replace(/^\s+/, '').replace(/\s+$/, '');
      break;
    case POSITION.center:
      while (str.match(/\w\s+\w/)) {
        str = str.replace(/(\w)(\s+)(\w)/, `$1$3`);
      }
      break;
    case POSITION.all:
      str = str.replace(/\s/g, '');
      break;
    default:
  }

  return str;
}

const result = trim(str);

console.log(`|${result}|`); //  |s t  r|
```

```js
Regex: string.replace(/\s/g, '');
join: string.split(' ').join('');
```

## Reference

[前端面试每日 3+1-以前端面试题来驱动学习，提倡每日学习与思考，每天进步一点！](http://www.h-camel.com/index.html)

[HTML - Free tutorial to learn HTML and CSS](https://marksheet.io/html-block-inline.html)
