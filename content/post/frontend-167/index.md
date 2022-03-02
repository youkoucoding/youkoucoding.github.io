---
title: '毎日のフロントエンド  167'
date: 2022-03-02T13:19:24+09:00
description: frontend 每日一练
categories:
  - HTML
  - CSS
  - JavaScript
---

## HTML

### Shadow DOM 和 Virtual DOM 区别

- Shadow DOM

  - Shadow DOM 是浏览器提供的一个可以允许将隐藏的 DOM 树添加到常规的 DOM 树中——它以 shadow root 为起始根节点，在这个根节点的下方，可以是任意元素，和普通的 DOM 元素一样

- Virtual DOM
  - 虚拟 DOM 是由 js 实现的避免 DOM 树频繁更新，通过 js 的对象模拟 DOM 中的节点，然后通过特定的 render 方法将它渲染成真实的节点，数据更新时，渲染得到新的 Virtual DOM，与上一次得到的 Virtual DOM 进行 diff，得到所有需要在 DOM 上进行的变更，然后在 patch 过程中应用到 DOM 上实现 UI 的同步更新

## CSS

### `linear-gradient`

- 概念：线性渐变,向下/向上/向左/向右/对角方向,为了创建一个线性渐变，至少定义两种颜色结点。颜色结点即呈现平稳过渡的颜色。同时，也可以设置一个起点和一个方向（或一个角度)

```css
/* 从上到下 */
#grad {
  background: linear-gradient(red, blue);
}
/* 从左到右 */
#grad {
  background: linear-gradient(to right, red, blue); /* 标准的语法 */
}
/* 对角 */
#grad {
  background: linear-gradient(to right, red, blue); /* 标准的语法 */
}
/* 使用角度 */
#grad {
  background: linear-gradient(180deg, red, blue); /* 标准的语法 */
}
```

## Tips

### css 布局总结

#### 单列布局

> 单列布局是最常用的一种布局，它的实现效果就是将一个元素作为布局容器，通常设置一个较小的（最大）宽度来保证不同像素宽度屏幕下显示一致。

- 一些网站会将单列布局与其他布局方式混合使用，比如拉勾网首页的海报和左侧标签就使用了 2 列布局，这样既能向下兼容窄屏幕，又能按照主次关系显示页面内容。
- 这种布局的优势在于基本上可以适配超过布局容器宽度的各种显示屏幕，比如上面的示例网站布局容器宽度为 700px，也就是说超过 700px 宽度的显示屏幕上浏览网站看到的效果是一致的
- 最大的缺点也是源于此，过度的冗余设计必然会带来浪费。例如，在上面的例子中，其实我的屏幕宽度是足够的，可以显示更多的内容，但是页面两侧却出现了大量空白区域，如果在 4k 甚至更宽的屏幕下，空白区域大小会超过页面内容区域大小

#### 2 列布局

> 效果是将页面分割成左右宽度**不等的两列**，宽度较小的列设置为固定宽度，剩余宽度由另一列撑满。为了描述方便，暂且称宽度较小的列父元素为次要布局容器，宽度较大的列父元素为主要布局容器

- 这种布局适用于内容上具有明显主次关系的网页，比如 API 文档页面中左侧显示内容导航，右侧显示文档描述；
- 又比如后台管理系统中左侧显示菜单栏，右侧显示配置页面。
- 相对于单列布局，在屏幕宽度适配方面处理得更好。当屏幕宽度不够时，主要内容布局容器优先显示，次要内容布局容器改为垂直方向显示或隐藏，但有时候也会和单列布局搭配使用，作为单列布局中的子布局使用

#### 3 列布局

> 3 列布局按照左中右的顺序进行排列，通常中间列最宽，左右两列次之

#### 布局实现

##### 单列布局

- 单列布局，通过将设置布局容器（最大）宽度以及**左右边距为 auto** 即可实现

##### 2 列和 3 列布局

- （1）为了保证主要布局容器优先级，应将主要布局容器写在次要布局容器之前。
- （2）将布局容器进行水平排列；
- （3）设置宽度，即次要容器宽度固定，主要容器撑满；
- （4）消除布局方式的副作用，如浮动造成的高度塌陷；
- （5）为了在窄屏下也能正常显示，可以通过媒体查询进行优化。

---

- 使用 flex 布局实现 2 列布局的例子

- 第 1 步，写好 HTML 结构

```html
<style>
  /* 给布局容器设置高度和颜色 */
  /* 主要布局容器写在次要布局容器之前 */
  main,
  aside {
    height: 100px;
  }
  main {
    background-color: #f09e5a;
  }
  aside {
    background-color: #c295cf;
  }
</style>
<div>
  <main>主要布局容器</main>
  <aside>次要布局容器</aside>
</div>
```

- 第 2 步，将布局容器水平排列：

```html
<style>
  .wrap {
    display: flex;
    flex-direction: row-reverse;
  }
  .main {
    flex: 1;
  }
  .aside {
    flex: 1;
  }
</style>
<div class="wrap">
  <main class="main">主要布局容器</main>
  <aside class="aside">次要布局容器</aside>
</div>
```

- 第 3 步，调整布局容器宽度：

```html
<style>
  .wrap {
    display: flex;
    flex-direction: row-reverse;
  }
  .main {
    flex: 1;
  }
  .aside {
    width: 200px;
  }
</style>
<div class="wrap">
  <main class="main">主要布局容器</main>
  <aside class="aside">次要布局容器</aside>
</div>
```

- 第 4 步，消除副作用，比如浮动造成的高度塌陷。由于使用 flex 布局没有副作用，所以不需要修改，代码和效果图同第 3 步。

- 第 5 步，增加媒体查询。

```html
<style>
  .wrap {
    display: flex;
    flex-direction: row-reverse;
    flex-wrap: wrap;
  }
  .main {
    flex: 1;
  }
  .aside {
    width: 200px;
  }
  @media only screen and (max-width: 1000px) {
    .wrap {
      flex-direction: row;
    }
    .main {
      flex: 100%;
    }
  }
</style>
<div class="wrap">
  <main class="main">主要布局容器</main>
  <aside class="aside">次要布局容器</aside>
</div>
```

---

- 3 列布局的例子(圣杯布局)

- 第 1 步，写好 HTML 结构

```html
<style>
  /* 为了方便查看，给布局容器设置高度和颜色 */
  .main,
  .left,
  .right {
    height: 100px;
  }
  .main {
    background-color: red;
  }
  .left {
    background-color: green;
  }
  .right {
    background-color: blue;
  }
</style>
<div class="wrap">
  <main class="main">main</main>
  <aside class="left">left</aside>
  <aside class="right">right</aside>
</div>
```

- 第 2 步，让布局容器水平排列：

```html
<style>
  .main,
  .left,
  .right {
    float: left;
  }
</style>
<div class="wrap">
  <main class="main">main</main>
  <aside class="left">left</aside>
  <aside class="right">right</aside>
</div>
```

- 第 3 步，调整宽度，将主要布局容器 main 撑满，次要布局容器 left 固定 300px，次要布局容器 right 固定 200px。
  - 这里如果直接设置的话，布局容器 left 和 right 都会换行，所以需要通过设置父元素 wrap 内边距来压缩主要布局 main 给次要布局容器留出空间。
  - 同时通过设置次要布局容器边距以及采用相对定位调整次要布局容器至两侧。

```html
<style>
  .main,
  .left,
  .right {
    float: left;
  }
  .wrap {
    padding: 0 200px 0 300px;
  }
  .main {
    width: 100%;
  }
  .left {
    width: 300px;
    position: relative;
    left: -300px;
    margin-left: -100%;
  }
  .right {
    position: relative;
    width: 200px;
    margin-left: -200px;
    right: -200px;
  }
</style>
<div class="wrap">
  <main class="main">main</main>
  <aside class="left">left</aside>
  <aside class="right">right</aside>
</div>
```

- 第 4 步，消除副作用。
  - 使用浮动会造成高度塌陷，如果在父元素后面添加新的元素就会产生这个问题
  - 可以通过伪类来清除浮动，同时减小页面宽度，还会发现次要布局容器 left 和 right 都换行了，这个副作用在第 5 步时进行消除。

```html
<style>
  .main,
  .left,
  .right {
    float: left;
  }
  .wrap {
    padding: 0 200px 0 300px;
  }
  .wrap::after {
    content: '';
    display: block;
    clear: both;
  }
  .main {
    width: 100%;
  }
  .left {
    width: 300px;
    position: relative;
    left: -300px;
    margin-left: -100%;
  }
  .right {
    position: relative;
    width: 200px;
    margin-left: -200px;
    right: -200px;
  }
</style>
<div class="wrap">
  <main class="main">main</main>
  <aside class="left">left</aside>
  <aside class="right">right</aside>
</div>
```

- 第 5 步，利用媒体查询调整页面宽度较小情况下的显示优先级。
  - 希望优先显示主要布局容器 main，其次是次要布局容器 left，最后是布局容器 right。

```html
<style>
  .main,
  .left,
  .right {
    float: left;
  }
  .wrap {
    padding: 0 200px 0 300px;
  }
  .wrap::after {
    content: '';
    display: block;
    clear: both;
  }
  .main {
    width: 100%;
  }
  .left {
    width: 300px;
    position: relative;
    left: -300px;
    margin-left: -100%;
  }
  .right {
    position: relative;
    width: 200px;
    margin-left: -200px;
    right: -200px;
  }
  @media only screen and (max-width: 1000px) {
    .wrap {
      padding: 0;
    }
    .left {
      left: 0;
      margin-left: 0;
    }
    .right {
      margin-left: 0;
      right: 0;
    }
  }
</style>
<div class="wrap">
  <main class="main">main</main>
  <aside class="left">left</aside>
  <aside class="right">right</aside>
</div>
```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[shadow DOM](https://developer.mozilla.org/zh-CN/docs/Web/Web_Components/Using_shadow_DOM)

[linear-gradient() - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/gradient/linear-gradient%28%29)
