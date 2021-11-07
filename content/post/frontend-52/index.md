---
title: '毎日のフロントエンド　52'
date: 2021-11-07T10:47:11+09:00
description: frontend 每日一练
image: frontend-52-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第五十二日

## HTML

### **Question** `Form`表单提交时为什么会刷新页面？怎么预防刷新

因为早期网页交互模型只能是浏览器提交数据给服务器，服务器做出响应重新返回一个页面，浏览器加载这个页面进行显示。早期前端没有编程式发送网络请求的 API，更没有前端路由管理的概念，所以表单提交跳转页面是广泛接受的方案。

不想刷新可以使用 JS 拦截 form 的 onsubmit 事件，阻止掉浏览器的默认行为，然后用 ajax/fetch 和后台交互。另一个偏方是使用 iframe 作为 form 的 target，不过 JS 处理方面不如让浏览器别管自己全手动发请求来得简单。

## CSS

### **Question** 要是`position`跟`display`、`overflow`、`float`这些特性相互叠加后会怎么样

#### `display`、`position` 和 `float` 的相互关系

`display` 属性规定元素应该生成的框的类型。`block` `inline`...

`position` 属性规定元素的定位类型。

- `absolute`表示生成绝对定位的元素，相对于 static 定位以外的第一个父元素进行定位；
- `fixed`成绝对定位的元素，相对于浏览器窗口进行定位；
- `relative`生成相对定位的元素，相对于其正常位置进行定位；
- `static` 默认值。没有定位，元素出现在正常的流中（忽略 top, bottom, left, right z-index 声明）。

`Float`也是是一种布局方式，它定义元素在哪个方向浮动。在布局过程中也经常会使用它来达到左右并排布局的效果。

- `position:absolute`和`position:fixed` 优先级最高，有它存在的时候，浮动不起作用，`display` 的值也需要调整；
- 其次，元素的 `float` 特性的值不是 `none` 的时候或者它是根元素的时候，调整 `display` 的值；
- 最后，非根元素，并且非浮动元素，并且非绝对定位的元素，`display` 特性值等同设置值。

1. `display` 的值为 `none`: `position` 和 `float` 不起作用, 浮动和定位无效
2. `position` 的值是 `absolute` 或 `fixed`: 浮动失效，`display` 会被按规则重置
3. `float` 的值不是 `none`: 浮动并且 'display' 会被按照转换设置

## JavaScript

### **Question** 什么是事件委托

Event Delegation is basically a pattern to handle events efficiently. Instead of adding an event listener to each and every similar element, we can add an event listener to a parent element and call an event on a particular target using the `.target` property of the event object.

Example without event delegation:

```js
const customUl = document.createElement('ul');

for (var i = 1; i <= 10; i++) {
  const newElement = document.createElement('li');
  newElement.textContent = 'This is line ' + i;
  newElement.addEventListener('click', () => {
    console.log('Responding');
  });
  customUI.appendChild(newElement);
}
```

The above code will associate the function with every `<li>` element.

Implementing the same functionalities with an alternate approach. In this approach, we will associate the same function with all event listeners. We are creating too many responding functions. **We could extract this function and just reference the function too many functions:**

```js
const customUl = document.createElement('ul');

function responding() {
  console.log('Responding');
}

for (var i = 1; i <= 10; i++) {
  const newElement = document.createElement('li');
  newElement.textContent = 'This is line ' + i;
  newElement.addEventListener('click', responding);
  customUI.appendChild(newElement);
}
```

In above approach, we still have too many event listners pointing to the same function.

Implementing the same functionalities using a single function and single event:

```js
const customUl = document.createElement('ul');

function responding() {
  console.log('Responding');
}

for (var i = 1; i <= 10; i++) {
  const newElement = document.createElement('li');
  newElement.textContent = 'This is line ' + i;
  customUI.appendChild(newElement);
}
customUI.addEventListener('click', responding);
```

Now there is a single event listener and a single responding function.
In the above-shown method, we have improved the performance, but we have lost access to individual `<li>` elements so to resolve this issue, we will use a technique called **event delegation**.

The event object has a special property call `.target` which will help us in getting access to individual `<li>` elements with the help of phases.

Steps:

- `<ul>` element is clicked.
- The event goes in the capturing phase.
- It reaches the target (`<li>` in our case).
- It switches to the bubbling phase.
- When it hits the `<ul>` element, it runs the event listener.
- Inside the listener function `event.target` is the element that was clicked.
- **`Event.target` provides us access to the `<li>` element that was clicked.**

The `.nodeName` property of the `.target` allows us to identify a specific node. If our parent element contains more than one child element then we can identify specific elements by using the `.nodeName` property.

```js
const customUl = document.createElement('ul');

function responding(evt) {
  if (evt.target.nodeName === 'li') console.log('Responding');
}

for (var i = 1; i <= 10; i++) {
  const newElement = document.createElement('li');
  newElement.textContent = 'This is line ' + i;
  customUI.appendChild(newElement);
}

customUI.addEventListener('click', responding);
```

---

当发生点击事件（或传播的任何其他事件）时：

- 事件从 window、document、根元素向下传播，并经过目标元素的祖先（捕获阶段）；
- 事件发生在目标（目标阶段）上；
- 最后，事件在目标祖先之间冒出气泡，直到根元素 document 和 window（冒泡阶段）。

该机制称为**事件传播**。

事件委托是一种有用的模式，因为你可以只需要用一个事件处理程序就能侦听多个元素上的事件。

使用事件委托需要三个步骤：

- 确定要监视事件的元素的父级元素
- 把将事件侦听器附加到父元素
- 用 event.target 选择目标元素

## Reference

[position 跟 display、margin collapse、overflow、float 这些特性相互叠加后会怎么样？](https://www.cnblogs.com/jiangtuzi/p/4128962.html)

[Event Delegation in JavaScript - GeeksforGeeks](https://www.geeksforgeeks.org/event-delegation-in-javascript/)

[浅析 JavaScript 中的事件委托 - SegmentFault 思否](https://segmentfault.com/a/1190000023563411)
