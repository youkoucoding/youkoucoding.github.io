---
title: '毎日のフロントエンド  132'
date: 2022-01-26T15:49:05+09:00
description: frontend 每日一练
image: frontend-132-cover.jpg
categories:
  - CSS
  - Javascript
---

# 第一百三十二日

## CSS

### 巧用伪类`::before ::after`

基本功能是在 CSS 渲染中向元素的内容前面和后面插入内容。

1. 动态在元素中添加字符串
   - 场景：当需要在列表、或者多处相同样式中增加一个不变的字符串时，我们可以使用它；

```css
.price::before {
  content: '¥';
}
```

2. 不改变元素尺寸的边框

   - 场景：在宽度为百分比的元素中，为此元素增加边框，此时会导致元素超过原有的百分比；
   - 例如：导航条宽度为文档的 100% ，刚好撑满文档，但是需要在周围增加 1px 的边框，导致导航条宽度超过了浏览器宽度；
   - 建议：中等程度使用，有替代方案；

   ```css
   .meun {
    width: 100%;
    height: 80px;
    position: relative;
   }
   .menu::before {
    content: ""
    position: absolute;
    left: 0;
    border-left: 1px solid #ccc;
   }
   .menu::after {
    content: ""
    position: absolute;
    left: 0;
    border-right: 1px solid #ccc;
   }

   ```

3. `0.5px` 细边框

   - 场景：很多简单的几何图形，我们可以通过它们只需要用一个样式就可以解决问题；
   - 例如：带尖角的气泡对话框、筛选组件的下拉箭头；
   - 建议：强烈推荐；

   ```css
   .select::after {
     content: '';
     position: absolute;
     top: 50%;
     margin-top: -7px;
     right: 10px;
     display: inline-block;
     border-left: 1px solid #000;
     border-bottom: 1px solid #000;
     width: 14px;
     height: 14px;
     transform: rotate(-45deg);
   }
   ```

4. 清除浮动
   - 场景：当一个元素在众多设置了浮动样式 float: left 的后面，但是又要另起一行时；
   ```css
   .container::before {
     content: '';
     display: table;
   }
   .container::after {
     clear: both;
   }
   ```

## JavaScript

### 事件的冒泡

当事件(event)触发在某个元素上时，如果这个事件绑定了方法那么这个方法会被执行，如果没有绑定方法或者被绑定的方法返回 true，那么这个事件会向其父级传播，一层一层直到最顶层即 document 或者 window，除非被认为的中断。

#### 冒泡机制

现代浏览器的冒泡机制基本一致，事件都是由最内层的元素网最外层元素冒泡，冒泡顺序：`child->paren->body->html->document->window`

#### 事件捕获

事件的捕获刚好和冒泡的方向相反，由最外层开始捕获，然后到最内层，捕获顺序：`window->document->html->body->paren->child`

#### DOM 的事件流

事件捕获优先发生而冒泡后发生，这样一来从捕获到冒泡形成了一组事件流。

通过`addEventListener(event,fn,useCapture)`这个方法给 DOM 绑定事件时，前两个参数很容易理解一个是事件名称`event`，第二个是触发方法`fn`，其中第三个参数是一个 bool 值，用来设置绑定的方法是在**事件捕获(`true`)**时执行还是**冒泡(`false`)**时执行，一般会设置 false，这样比较安全。

#### 阻止事件冒泡

通常情况下，我们不会去做阻止事件冒泡的事情，但是有时候当我们不想同时执行绑定在两个 DOM 元素上的事件时，我们需要手动的阻止事件的冒泡，通常我们使用如下几种方式来阻止：

- `return false`：阻止默认事件和冒泡事件；
- `event.preventDefault()`：阻止默认事件但是允许冒泡事件；
- `event.stopPropagation()`：阻止冒泡但是允许默认事件；

默认事件：该元素默认执行的动作。例如：button 的默认事件是 submit，a 的默认事件是打开链接 等等...

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[lijiakof/frontend-book](https://github.com/lijiakof/frontend-book)
