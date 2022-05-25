---
title: '毎日のフロントエンド 286~288'
date: 2022-05-24T22:13:40+09:00
description: frontend 每日一练
categories:
  - HTML
  - Tips
---

## CSS

### css 属性恢复为初始化状态

- `all:unset`

### 移动端 `1px` 像素问题及解决办法

#### 为什么会有 1px 问题

- **物理像素-设备像素 & 逻辑像素-CSS 像素**
  - 物理像素：移动设备出厂时，不同设备自带的不同像素，也称硬件像素；
  - 逻辑像素：css 中记录的像素
- 移动端 CSS 里面写了 1px，实际上看起来比 1px 粗；
  - 设计师要求的 1px 是指设备的物理像素 1px，而 CSS 里记录的像素是逻辑像素，它们之间存在一个比例关系，通常可以用 javascript 中的 `window.devicePixelRatio` 来获取
  - 也可以用媒体查询的 `-webkit-min-device-pixel-ratio` 来获取
  - iPhone 的 `devicePixelRatio==2`, 而 `border-width: 1px;` 描述的是设备独立像素，所以，border 被放大到物理像素 2px 显示，在 iPhone 上就显得较粗

#### 解决方案

##### 1. `transform: scale(0.5)` 方案 - 推荐: 很灵活

- 设置`height: 1px`，根据媒体查询结合`transform`缩放为相应尺寸

```css
div {
  height: 1px;
  background: #000;
  -webkit-transform: scaleY(0.5);
  -webkit-transform-origin: 0 0;
  overflow: hidden;
}
```

- 用`::after`和`::before`,设置`border-bottom：1px solid #000`,然后在缩放`-webkit-transform: scaleY(0.5)`;可以实现两根边线的需求

```css
div::after {
  content: '';
  width: 100%;
  border-bottom: 1px solid #000;
  transform: scaleY(0.5);
}
```

- 用`::after`设置`border：1px solid #000;` `width:200%;` `height:200%`,然后再缩放`scaleY(0.5)`; 优点可以实现圆角，缺点是按钮添加 active 比较麻烦

```css
.div::after {
  content: '';
  width: 200%;
  height: 200%;
  position: absolute;
  top: 0;
  left: 0;
  border: 1px solid #bfbfbf;
  border-radius: 4px;
  -webkit-transform: scale(0.5, 0.5);
  transform: scale(0.5, 0.5);
  -webkit-transform-origin: top left;
}
```

##### 2. 媒体查询 + `transfrom`

```css
/* 2倍屏 */
@media only screen and (-webkit-min-device-pixel-ratio: 2) {
  .border-bottom::after {
    -webkit-transform: scaleY(0.5);
    transform: scaleY(0.5);
  }
}
/* 3倍屏 */
@media only screen and (-webkit-min-device-pixel-ratio: 3) {
  .border-bottom::after {
    -webkit-transform: scaleY(0.33);
    transform: scaleY(0.33);
  }
}
```

##### 3. `viewport`+`rem`实现

```html
<html style="font-size:18px;">
  <head>
    <title>1px question</title>
    <meta http-equiv="Content-Type" content="text/html;charset=UTF-8" />
    <meta
      name="viewport"
      id="WebViewport"
      content="initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no"
    />
    <style>
      html {
        font-size: 1px;
      }
      * {
        padding: 0;
        margin: 0;
      }
      .bds_b {
        border-bottom: 1px solid #ccc;
      }
      .a,
      .b {
        margin-top: 1rem;
        padding: 1rem;
        font-size: 1.4rem;
      }
      .a {
        width: 30rem;
      }
      .b {
        background: #f5f5f5;
        width: 20rem;
      }
    </style>
    <script>
      var viewport = document.querySelector('meta[name=viewport]');
      //下面是根据设备像素设置viewport
      if (window.devicePixelRatio == 1) {
        viewport.setAttribute(
          'content',
          'width=device-width,initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no'
        );
      }
      if (window.devicePixelRatio == 2) {
        viewport.setAttribute(
          'content',
          'width=device-width,initial-scale=0.5, maximum-scale=0.5, minimum-scale=0.5, user-scalable=no'
        );
      }
      if (window.devicePixelRatio == 3) {
        viewport.setAttribute(
          'content',
          'width=device-width,initial-scale=0.3333333333333333, maximum-scale=0.3333333333333333, minimum-scale=0.3333333333333333, user-scalable=no'
        );
      }
      var docEl = document.documentElement;
      var fontsize = 10 * (docEl.clientWidth / 320) + 'px';
      docEl.style.fontSize = fontsize;
    </script>
  </head>
  <body>
    <div class="bds_b a">下面的底边宽度是虚拟1像素的</div>
    <div class="b">上面的边框宽度是虚拟1像素的</div>
  </body>
</html>
```

##### 4. `box-shadow` 实现

- `box-shadow`属性的用法：`box-shadow: h-shadow v-shadow [blur] [spread] [color] [inset]`

- 参数分别表示: 水平阴影位置，垂直阴影位置，模糊距离，阴影尺寸，阴影颜色，将外部阴影改为内部阴影，后四个可选。
- 设置成`-1px` 是为了让阴影尺寸稍小于 div 元素尺寸，这样左右两边的阴影就不会暴露出来，实现只有底部一边有阴影的效果。从而实现分割线效果（单边边框）。

```css
div {
  -webkit-box-shadow: 0 1px 1px -1px rgba(0, 0, 0, 0.5);
}
```

## Reference

- [fe-interview/history.md](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

- [all - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/all)

- [移动端 1px 像素问题及解决办法](https://segmentfault.com/a/1190000040393256)
