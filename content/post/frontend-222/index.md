---
title: '毎日のフロントエンド 222'
date: 2022-05-02T23:24:53+09:00
description: frontend 每日一练
categories:
  - HTML
  - CSS
  - Tips
---

## HTML

### HTML5 对元素内容进行拼写检查

- `spellcheck` 定义是否可以检查元素的拼写错误
  - `true`: 设置在可能的情况下会去检查元素内容的拼写错误;
  - `false`: 设置在可能的情况下关闭对元素内容拼写检查。
- 如果没有设置这个属性，默认值由元素自身类型和浏览器设置决定。默认值也可以被继承，当有祖先元素的 spellcheck 设置为 true 的情况下，子元素的默认值也是 true。

ps:

- `contenteditable`: 是一个枚举属性，表示元素是否可被用户编辑。如果可以，浏览器会修改元素的部件以允许编辑:
  - `true` 或空字符串，表示元素是可编辑的
  - `false` 表示元素不是可编辑的

## CSS

### css 实现饼图

方法一： 使用伪元素 + transform + css 渐变实现

```css
.pie {
  width: 100px;
  height: 100px;
  border-radius: 50%;
  background: yellowgreen;
  background-image: linear-gradient(to right, transparent 50%, #655 0);
}

.pie::before {
  content: '';
  display: block;
  margin-left: 50%;
  height: 100%;
  border-radius: 0 100% 100% 0 / 50%;
  background-color: inherit;
  transform-origin: left;
  transform: rotate(0.3turn);
}
```

方法二： svg

```html
<svg width="100" height="100">
  <circle r="25" cx="50" cy="50" />
</svg>
```

```css
circle {
  fill: yellowgreen;
  stroke: #655;
  stroke-width: 50;
  stroke-dasharray: 60 158;
}

svg {
  transform: rotate(-90deg);
  background: yellowgreen;
  border-radius: 50%;
}
```

方法三： 角向渐变 `conic-gradient`

```css
.pie {
  width: 200px;
  height: 200px;
  border-radius: 50%;
  background: conic-gradient(#2ecc7e 0 30%, #abcdef 30% 60%, pink 60% 100%);
}
```

## Tips

### 异步加载和延迟加载

- **异步加载：`async`**

  - 浏览器会在解析 HTML 的同时进行加载 javascript，一旦该 javascript 加载完毕，浏览器会暂停 HTML 的加载并执行 javascript，之后继续 HTML 的加载

- **延迟加载： `defer`**

  - 同样是解析 HTML 的同时进行加载 javascript，但是浏览器会等待 HTML 全部解析完毕后再进行执行已加载的 javascript

## Reference

- [haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

- [spellcheck - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Global_attributes/spellcheck)

- [contenteditable - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Global_attributes/contenteditable)

- [`<script>` `async` & `defer` | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/script#attr-async)

- [CSS 引入方式和优先级简单介绍](https://www.jianshu.com/p/a27e334180ba)
