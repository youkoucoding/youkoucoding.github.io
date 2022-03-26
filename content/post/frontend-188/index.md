---
title: '毎日のフロントエンド 188'
date: 2022-03-26T17:20:33+09:00
description: frontend 每日一练
categories:
  - HTML
  - CSS
  - JavaScript
  - Tips
---

## HTML

### PNG 格式

#### 什么是 PNG

- PNG 的全称叫便携式网络图型（Portable Network Graphics）是目前最流行的网络传输和展示的图片格式，原因有如下几点：
  - _无损压缩_：PNG 图片采取了基于 LZ77 派生算法对文件进行压缩，使得它压缩比率更高，生成的文件体积更小，并且不损失数据。
  - _体积小_：它利用特殊的编码方法标记重复出现的数据，使得同样格式的图片，PNG 图片文件的体积更小。网络通讯中因受带宽制约，在保证图片清晰、逼真的前提下，优先选择 PNG 格式的图片。
  - _支持透明效果_：PNG 支持对原图像定义 256 个透明层次，使得图像的边缘能与任何背景平滑融合，这种功能是 GIF 和 JPEG 没有的。

#### PNG 类型

- `PNG 8`：PNG 8 中的 8，其实指的是 8bits，相当于用 2^8（2 的 8 次方）大小来存储一张图片的颜色种类，2^8 等于 256，也就是说 PNG 8 能存储 256 种颜色，一张图片如果颜色种类很少，将它设置成 PNG 8 得图片类型是非常适合的。

- `PNG 24`：PNG 24 中的 24，相当于 3 乘以 8 等于 24，就是用三个 8bits 分别去表示 R（红）、G（绿）、B（蓝）。R(0~255),G(0~255),B(0~255)，可以表达 256 乘以 256 乘以 256=16777216 种颜色的图片，这样 PNG 24 就能比 PNG 8 表示色彩更丰富的图片。但是所占用的空间相对就更大了。

- `PNG 32`：PNG 32 中的 32，相当于 PNG 24 加上 8bits 的透明颜色通道，就相当于 R（红）、G（绿）、B（蓝）、A（透明）。R(0~255),G(0~255),B(0~255),A(0~255)。比 PNG 24 多了一个 A（透明），也就是说 PNG 32 能表示跟 PNG 24 一样多的色彩，并且还支持 256 种透明的颜色，能表示更加丰富的图片颜色类型。

---

- png8 和 png24 的根本区别，不是颜色位的区别，而是存储方式不同。 “PNG8”是指 8 位索引色位图，“PNG24”是 24 位索引色位图
- png8 有 1 位的布尔透明通道（要么完全透明，要么完全不透明，不支持半透明），png24 则有 8 位（256 阶）的布尔透明通道（所谓半透明）

- 保存什么样的格式：
  - 色彩丰富的、大的图片 `jpg`
  - 尺寸小的，色彩不丰富的和背景透明的 `gif` 或者 `png8`
  - 半透明`png24`

## CSS

### css 给一个元素加边框

```css
:scope {
  border: 3px solid black;

  box-shadow: 0 0 0 1px black; /*不影响布局,无限叠加*/

  outline: 1px solid black; /*不支持圆角*/

  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg'%3E%3Crect width='100%25' height='100%25' stroke='%23000' fill='transparent'/%3E%3C/svg%3E");

  background-clip: content-box; /*形成透明边框*/
  padding: 1px;

  border-image: linear-gradient(red, black) 1;
  border: 1px solid;
}
```

## JavaScript

### `stopPropagation()` 和 `preventDefault()` 区别

- `stopPropagation()` **阻止事件冒泡**，即冒泡事件到当前元素处就终止了，不会继续向上级元素传递。

- `preventDefault()` **阻止默认事件**，例如：在 a 标签的 click 事件中执行了该方法，则不会进行默认的链接跳转。

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[`:scope` - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:scope)

[event.stopPropagation - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/stopPropagation)

[event.preventDefault - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/preventDefault)
