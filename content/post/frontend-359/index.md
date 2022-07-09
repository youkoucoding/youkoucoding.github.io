---
title: '毎日のフロントエンド 359'
date: 2022-07-08T23:28:05+09:00
description: frontend 每日一练
categories:
  - CSS
  - JS
  - Tips
---

## Tips

### 组件、模块、元素

#### 组件

- 一般为自定义的有比较高的复用性，比较广泛的通用性的一段代码或多段代码组成。例如： 表单组件 Table 组件，轮播组件等，都是有比较高的复用和通用效果。

#### 模块

- 一般是为了更好的可维护，更清晰的结构，根据整体切分出的组成部分，同时也提高复用性，但一般模块的通用性不如组件。
  模块可以会包含一个或多个组件。

#### 元素

- 元素一般是指向比较具体的某一项，比较简单的个体，例如一个 HTML 标签，一张图片，一个音视频文件。

### 不再需要 JS 做的 5 件事

#### 1. 使用 css 控制 svg 动画

- 用 css 控制 svg 产生动画效果

```css
.trail {
  stroke-width: 2;
  stroke-dasharray: 1 10 5 10 10 5 30 150;
  animation-name: trail;
  animation-timing-function: ease-out;
}

@keyframes trail {
  from,
  20% {
    stroke-width: 3;
    stroke-dashoffset: 80;
  }
  100%,
  to {
    stroke-width: 0.5;
    stroke-dashoffset: -150;
  }
}
```

#### 2. sidebar 使用 css 实现 hover 时才出现的侧边栏

```css
nav {
  position: 'absolute';
  right: 100%;
  transition: 0.2s transform;
}

nav:hover,
nav:focus-within {
  transform: translateX(100%);
}
```

- `hover` 时设置 transform 属性可以让元素偏移，且 translateX(100%) 可以位移当前元素宽度的身位

#### 3. sticky position

```css
.square {
  position: sticky;
  top: 2em;
}
```

- 该元素会始终展示在其父容器内，但一旦其出现在视窗时，当 top 超过 2em 后就会变为 fixed 定位并保持原位。

#### 4. 手风琴菜单

```css
<details>
  <summary>title</summary>
  <p>1</p>
  <p>2</p>
</details>
```

- `<details>` 标签内的 `<summary>` 标签内容总是会展示，且点击后会切换 `<details>` 内其他元素的显隐藏。虽然这做不了特殊动画效果，但如果只为了做一个普通的展开折叠功能，用 HTML 标签就够了

#### 5. 暗色主题

```css
@media (prefers-color-scheme: light) {
  /** ... */
}
@media (prefers-color-scheme: dark) {
  /** ... */
}
@media (prefers-color-scheme: no-preference) {
  /** ... */
}
```

- 如果使用 Checkbox 勾选是否开启暗色主题，也可以仅用 CSS 变量判断，核心代码是：

```css
#checkboxId:checked ~ .container {
  background-color: black;
}
```

- `~` 这个符号表示，`selector1 ~ selector2` 时，为选择器 `selector1` 之后满足 `selector2` 条件的兄弟节点设置样式

#### 6. 幻灯片滚动

```css
html {
  scroll-snap-type: y mandatory;
}
.child {
  scroll-snap-align: start;
}
```

- 页面设置为精准捕捉子元素滚动位置，在滚轮触发、鼠标点击滚动条松手或者键盘上下按键时，scroll-snap-type: y mandatory 可以精准捕捉这一垂直滚动行为，并将子元素完全滚动到可视区域。

#### 7. 颜色选择器

```html
<input type="color" value="#000000" />
```

- 该选择器的好处是性能、可维护性都非常非常的好，甚至可以捕捉桌面的颜色，不好的地方是无法对拾色器进行定制

## Reference

- [fe-interview/history.md](https://github.com/haizlin/fe-interview/blob/master/category/history.md)
- [5 things you don't need Javascript for](https://lexoral.com/blog/you-dont-need-js/)
- [`<details>`：详细信息展现元素 - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/details)
- [prefers-color-scheme - CSS: Cascading Style Sheets | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-color-scheme)
- [weekly/238.精读《不再需要 JS 做的 5 件事》](https://github.com/ascoders/weekly/blob/master/%E5%89%8D%E6%B2%BF%E6%8A%80%E6%9C%AF/238.%E7%B2%BE%E8%AF%BB%E3%80%8A%E4%B8%8D%E5%86%8D%E9%9C%80%E8%A6%81%20JS%20%E5%81%9A%E7%9A%84%205%20%E4%BB%B6%E4%BA%8B%E3%80%8B.md)
