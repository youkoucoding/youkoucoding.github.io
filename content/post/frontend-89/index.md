---
title: '毎日のフロントエンド  89'
date: 2021-12-14T12:28:13+09:00
description: frontend 每日一练
image: frontend-89-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第八十九日

## HTML

### **Question:** a 标签的`href`和`onclick`属性同时存在时哪个先触发

`onclick`先触发，判断依据是在`onclick`中使用`preventDefault`方法可以阻止`a`标签的跳转，说明 a 标签的跳转行为是一个默认行为

## CSS

### **Question:** 外层有一个自适应高度的 div，里面有两个 div，一个高度固定 300px，另一个如何填满剩余的高度

- 设置外层自适应高度的容器为`flex`布局，利用`flex-basis`属性即可实现自动填满剩余高度

```css
.container {
  display: flex;
  flex-flow: column nowrap;
  height: 500px;
  border: 2px dashed orange;
}
.area1 {
  flex-basis: 300px;
  background-color: lightblue;
}
.area2 {
  flex: 1;
  background-color: darkcyan;
}
```

---

```css
height: calc(100% - 300px);
```

## JavaScript

### **Question:** `js`异步加载有哪些方案

1. 将`script`标签放在`body`结束标签之前

   - 会先加载 dom 树，然后再加载 js 脚本

```html
<html>
  <head></head>
  <body>
    .....
    <script type="text/javascript" src="..."></script>
  </body>
</html>
```

2. 在`onload`方法中给`dom`树动态添加`script`标签

   - 先加载 dom 树，然后触发 onload 方法添加 script 标签加载 js 脚本

```html
<html>
  <head></head>
  <body
    onload="() => {
  var element = document.creatElement('script');
  element.type = 'text/javascript';
  element.src = '...';
  var headTag = document.getElementsByTagName('head')[0];
  headTag.insertBefore(element, headTag.firstChild);
}"
  >
    .....
  </body>
</html>
```

3. `defer`属性(attribute)

   - 并行加载 dom 树和下载 js 脚本，js 脚本下载后会**等`dom`树解析完再执行**

```js
<html>
  <head>
    <script defer type='text/javascript'></script>
  </head>
  <body>.....</body>
</html>
```

4. `async`属性

   - 这种方案也会并行加载 dom 树和下载 js 脚本，**js 脚本下载完后立刻并行执行**

```html
<html>
  <head>
    <script async type="text/javascript"></script>
  </head>
  <body>
    .....
  </body>
</html>
```

5. `<script type="module">`（ `defer` 效果） 和 动态加载模块的 `import()` 函数

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)
