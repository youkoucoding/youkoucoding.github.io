---
title: '毎日のフロントエンド  97'
date: 2021-12-22T16:21:09+09:00
description: frontend 每日一练
image: frontend-97-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第九十七日

## HTML

### **Question:** `DOCTYPE` 有什么作用

`DOCTYPE`声明指定了浏览器对于 HTML 文档解析的类型；

- `HTML5`的`DOCTYPE`只有一种：
  - `<!DOCTYPE html>`:在 `HTML5`中，`DOCTYPE` 唯一的作用是启用标准模式。更早期的 `HTML` 标准会附加其他意义，但没有任何浏览器会将 `DOCTYPE` 用于怪异模式和标准模式之间互换以外的用途。
- `HTML4.01`的`DOCTYPE`有三种：
  - `Strict`是不包括展示性和废弃的属性 以及框架集`framset`
  - `Transitional` 包括展示性和废弃属性 不包含框架集
  - `Framset` 在`Transitional` 基础上包括框架集

NOTE:

- 位置必须放在文档顶端，任何放在 DOCTYPE 前面的东西，比如批注或 XML 声明，会令 IE9 或更早期的浏览器触发怪异模式
- DOCTYPE 不是 HTML 标签，它是一个它是浏览器模式渲染的指令，更没有结束标签
- `<!DOCTYPE>` 声明**大小写不敏感**

## CSS

### **Question:** 如何更改`placeholder`的字体颜色和大小

```css
  <style>
    /* Chrome浏览器 */
    input::-webkit-input-placeholder {
      color: red;
    }

    /* 火狐浏览器 */
    input::-moz-placeholder {
      color: red;
    }

    /* IE */
    input:-ms-input-placeholder {
      color: red;
    }
  </style>
<body>
  <input type="text" placeholder="你好">
</body>
```

## JavaScript

### **Question:** 如何给 li 绑定事件（`ul`下有 1000+个`li`）

`for`循环遍历所有`li`，然后添加事件,引起大量浏览器重绘与重排。

> 当子元素过多时，可以利用“**事件冒泡**”在 `ul`(共同的父元素) 上绑定事件，实现**事件委托**。可以利用 `event.target` 对被触发的子元素进行操作。

事件冒泡： 事件从最深的节点开始，然后逐步向上传播事件。

给最外面的`div`加点击事件，那么子元素 `ul`，`li`，`a`等做点击事件的时候，都会冒泡到最外层的`div`上，都会触发，委托它们父级**代为执行事件**。

```js
document.getElementsByTag('ul')[0].addEventListener('event', (e) => {
  /**
   *  利用 e.target 对冒泡上来的元素做区分
   *  e.target.nodeName, e.target.id
   */
});
```

---

#### 事件流

- 事件捕获
  - 父元素到子元素传递
- 事件冒泡

  - 子元素向父元素传递

- 具体什么时候执行函数是通过 `addEventListener()`**第三个参数**决定的，不填默认为 `false`，也就是冒泡时执行，为 `true`，捕获时执行。

#### 事件模型

- 原始事件模型 （没有事件流）
- IE 事件模型 （IE 浏览器独有）
- DOM2 事件模型 （W3C 规范的标准事件模型）_现代浏览器的标准_

**DOM2 事件模型**事件流分为三个阶段：

- 捕获事件阶段
- 处于目标阶段
- 冒泡事件阶段

例：

点击 子元素 `li` 时：

- 首先是捕获阶段（`document-->html-->body-->div-->ul-->li`）
- 然后是处于目标阶段，也就是到达点击的 `li`，处于捕获和冒泡的中间
- 最后是冒泡阶段（`li-->ul-->div-->body-->html-->document`）

---

#### 应用场景

##### 冒泡

_「下拉加载列表」的功能_，循环遍历出列表的数据，并逐条添加点击事件。会占用大量的内存，从而影响网页的性能。

利用「冒泡事件流」的特性来，解决这个问题。只要在「ul」或者「div」等父元素中添加点击事件。

```html
<div class="list-container">
  <ul class="list"></ul>
</div>
<button class="btn">添加列表项</button>

<script>
  let list = document.getElementsByClassName('list')[0]; //ul
  let listItem = document.querySelector('.list-item');
  let btn = document.querySelector('.btn'); //button
  let dataList = ['1', '2', '3', '4', '5'];

  let addDOM = (text) => {
    let newDOM = document.createElement('li');
    newDOM.classList.add('list-item');
    //添加自定义属性，可以存接口的需要的属性值，例如:查看该项数据详细的ID
    newDOM.setAttribute('data-text', text);
    newDOM.innerHTML = text;
    list.appendChild(newDOM);
  };

  dataList.forEach((x, i) => {
    addDOM(x);
  });

  list.addEventListener('click', (e) => {
    console.log(e.target.dataset.text); //获取自定义属性 data-text 的值
  });

  btn.addEventListener('click', () => {
    dataList.push(dataList.length + 1);
    addDOM(dataList[dataList.length - 1]);
  });
</script>
```

假设现在父元素和子元素中都绑定了点击事件，点击子元素的时候，父元素的点击事件也跟着触发了。如果只希望目标元素的事件触发，就需要阻止冒泡。可以使用`event.stopPropagation()`来阻止事件冒泡。

```html
<div class="list-container">
  <ul class="list">
    <li class="list-item">1</li>
  </ul>
</div>
<script>
  let listContainer = document.getElementsByClassName('list-container')[0]; //div
  let list = document.getElementsByClassName('list')[0]; //ul
  let listItem = document.querySelector('.list-item'); //li

  let doSome = (text) => {
    console.log(text);
  };
  listItem.addEventListener('click', (e) => {
    //true 自定义的参数「e」和「event」对象引用是相同的。
    console.log('说明使用e和event是等价的，可以相互使用', e === event);
    e.stopPropagation(); //阻止事件传递
    doSome('li————最先执行');
  });
  list.addEventListener('click', (e) => {
    console.log('——————ul没有阻止冒泡——————');
    doSome('ul————接着执行');
  });
  listContainer.addEventListener('click', () => {
    doSome('div————最后执行');
  });
</script>
```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[前端进阶](https://muyiy.cn/)

[伪元素表单控件默认样式重置与自定义大全 « 张鑫旭-鑫空间-鑫生活](https://www.zhangxinxu.com/wordpress/2013/06/%e4%bc%aa%e5%85%83%e7%b4%a0-%e8%a1%a8%e5%8d%95%e6%a0%b7%e5%bc%8f-pseudo-elements-style-form-controls/)

[JS 中的事件、事件冒泡和事件捕获、事件委托](https://www.cnblogs.com/leftJS/p/10948138.html)
