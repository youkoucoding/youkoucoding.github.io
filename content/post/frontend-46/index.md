---
title: '毎日のフロントエンド　46'
date: 2021-11-01T15:01:13+09:00
description: frontend 每日一练
image: frontend-46-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第四十六日

## CSS

### **Question:** 对媒体查询的理解

#### `CSS3`中的媒体查询

- `width` – 输出设备渲染区域（如可视区域的宽度或打印机纸盒的宽度）的宽度
- `height` – 输出设备渲染区域（如可视区域的高度或打印机纸盒的高度）的高度
- `device-width` – 输出设备的宽度（整个屏幕或页的高度，而不是仅是渲染区域）
- `device-height` – 输出设备的高度（整个屏幕或页的高度，而不是仅是渲染区域）
- `orientation` – 设备处于横屏（宽度大于高度）模式还是竖屏（高度大于宽度）模式
- `aspect-ratio` – 输出设备目标显示区域的宽高比
- `device-aspect-ratio` – 输出设备的宽高比
- `resolution` – 输出设备的分辨率（像素密度）
- `color` – 检查设备支持多少种颜色等
- `color-index` – 输出设备中颜色查询表中的条目数量
- `monochrome` – 指定了一个黑白（灰度）设备每个像素的比特数
- `scan` – 检查电视输出设备是顺序扫描还是隔行扫描
- `grid` – 判断输出设备是网格设备还是位图设备

#### 基本语法

媒体查询最基本的形式，就是单独或组合使用媒体类型和媒体特性(后者要置于括号中)，如：

```css
@media screen {
    body {
        font-size: 20px;
    }
}

@media screen, print {
    body {
        font-size: 20px;
    }
}

@media (width: 30em) {
    nav li {
        display: block;
    }
}

@media screen and (width: 30em) {
    nav li {
        display: block;
    }
}
```

#### 嵌套

```css
/*例子1:媒体类型套媒体特性*/
@media screen {
    @media (min-width: 20em) {
        img {
            display: block;
            width: 100%;
            height: auto;
        }
    }
    @media (min-width: 40em) {
        img {
            display: inline-block;
            max-width: 300px;
        }
    }
}

/*例子2:媒体特性多层嵌套*/
@media (hover: on-demand) {
    @media (pointer: coarse) {
        input[type=checkbox] ~ label {
            padding: .5em;
        }
    }
    @media (pointer: fine) {
        input[type=checkbox] ~ label {
            padding: .1em;
        }
    }
}
```

#### 否定式查询

可以用关键字`not`表示一个否定查询； `not`必须置于查询的一开头并会对整条查询串生效，除非逗号分割的多条

```css
@media not print {
    body {
        background: url('paisley.png');
    }
}

/*否定`print and (min-resolution: 1.5dppx)`这一整个条件*/
@media not print and (min-resolution: 1.5dppx) {
    .external {
        background: url('arrow-lowres.png');
    }
}

/* not A 或 not B */
@media not (hover: hover), not (pointer: coarse) {
    font-size: 20px;
}
```

#### 根据媒体特性的范围查询

指定一个固定的宽度通常是没有意义的，更多的情况下，我们需要限定的是类似“小于等于”或“大于等于”这样的范围，而大多数媒体特性可以通过添加`max-`和`min-`前缀达到上述目的

```css
/*0 至 30em*/
@media (max-width: 30em) {
    nav li {
        display: block;
    }
}

/*30em 至 100em*/
@media (min-width: 30em) and (max-width: 100em)  {
    nav li {
        display: block;
    }
}
```

媒体查询**不仅**是为了适应终端尺寸的， 比如：打印的时候是不需要打印一些只需要体现在网页上的元素, `media query`都可以解决。

```css
@media print {
  .site-footer-credits {
    display: none;
  }
  .noprint {
    display: none;
  }
  .page-header {
    text-align: left;
  }
}
```

## JavaScript

### **Question:** 写一个使两个整数进行交换的方法（不能使用临时变量）

利用执行顺序

```js
a = a + b;
b = a - b;
a = a - b;
```

异或取值

```js
a ^= b;
b ^= a;
a ^= b;
```

---

```js
let a = 1,
  b = 2;
[a, b] = [b, a];
```

## Reference

[haizlin/fe-interview: HTML/CSS/JavaScript/Vue/React/Nodejs/TypeScript/ECMAScritpt/Webpack/Jquery/小程序/软技能……](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)

[使用媒体查询 - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Media_Queries/Using_media_queries)

[ 全面理解 CSS 媒体查询 - 掘金](https://juejin.cn/post/6844903486258216967)
