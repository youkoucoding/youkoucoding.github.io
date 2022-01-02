---
title: '毎日のフロントエンド  108'
date: 2022-01-02T11:52:07+09:00
description: frontend 每日一练
image: frontend-108-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第一百零八日

## HTML

### **Question:** 在默认的情况下，使用 h1 标签呈现出什么效果

`h1`标签

- 默认： 加粗 块状元素
- 字体大小：`font-size:2em;` 未经过调整的浏览器大小是 32px

## CSS

### **Question:** position 的 relative 和 absolute 定位原点

两者的定位原点都是其**包含块区域的左上角**

`containing block`:用来确定和影响元素的尺寸和位置属性的矩形区域；

- The size and position of an element are often impacted by its containing block. Percentage values that are applied to the width, height, padding, margin, and offset properties of an absolutely positioned element.

一个元素的包含块完全受其 position 属性值的影响：

1. `static`或`relative`:
   - 最近的块级（`display`属性值为`block`，`inline-block`或`list-item`）祖先元素的`content-box`区域；static 情况下的原点
   - 或者最近的建立格式上下文的祖先元素，比如：table 容器，flex 容器，grid 容器或块级容器。
2. `absolute`
   - 最近的非 static（`fixed`, `absolute`, `relative`, or `sticky`）祖先元素的`padding-box`区域
3. `fixed`
   - 可视窗口 viewport 本身（属于 continuous media 类型时）或页面区域 page area（属于 paged media 类型时），即**初始包含块**；
4. 当属性值为 fixed 或 absolute 时，其包含块还有可能是最近的含有 transform 或 perspective 值不为 none 的祖先元素的 padding-box 区域。

> html 元素的包含块叫做初始包含块（`initial containing block`），它具有可视窗口（用于连续媒体）或页面区域（用于分页媒体）的尺寸

## JavaScript

### **Question:** Async/Await 如何通过同步的方式实现异步

`async/await` 是参照 `Generator` 封装的一套异步处理方案，可以理解为 Generator 的语法糖

#### 单向链表

> 链表（Linked list）是一种常见的基础数据结构，是一种线性表，但是并不会按线性的顺序储存数据，而是在每一个节点里存到下一个节点的指针（Pointer）。由于不必须按顺序储存，链表在**插入的时候可以达到 o(1)的复杂度**，比另一种线性表顺序表快得多，但是查找一个节点或者访问特定编号的节点则需要 o(n)的时间，而顺序表响应的时间复杂度分别是 o(logn)和 o(1)。

- 无需预先分配内存
- 插入/删除节点不影响其他节点，效率高（典型的例子：git commit、dom 操作）
- 单向链表：是链表中最简单的一种，它包含两个域，一个信息域和一个指针域
  - 单向链表特点：节点的链接方向是单向的；相对于数组来说，单链表的的随机访问速度较慢，但是单链表删除/添加数据的效率很高。

#### 迭代器 `Iterator`

- 创建一个指针对象，指向当前数据结构的起始位置
- 第一次调用指针对象的 next 方法，将指针指向数据结构的第一个成员
- 第二次调用指针对象的 next 方法，将指针指向数据结构的第二个成员
- 不断的调用指针对象的 next 方法，直到它指向数据结构的结束位置

一个对象要变成可迭代的，必须实现 `@@iterator` 方法，即对象（或它原型链上的某个对象）必须有一个名字是 `Symbol.iterator` 的属性( 返回一个对象的无参函数，被返回对象符合迭代器协议)（**原生具有该属性的有：字符串、数组、类数组的对象、`Set` 和 `Map`**）：

#### 生成器 `Generator`

Generator：生成器对象是生成器函数（GeneratorFunction）返回的，它符合可迭代协议和迭代器协议，既是迭代器也是可迭代对象，可以调用 next 方法，但它不是函数，更不是构造函数

- 生成器函数（GeneratorFunction）:

```js
function* name([param[, param[, ... param]]]) { statements }
```

调用一个生成器函数并不会马上执行它里面的语句，而是返回一个这个生成器的迭代器对象，当这个迭代器的 next() 方法被首次（后续）调用时，其内的语句会执行到第一个（后续）出现 yield 的位置为止（让执行处于暂停状），yield 后紧跟迭代器要返回的值。或者如果用的是 yield\*（多了个星号），则表示将执行权移交给另一个生成器函数（当前生成器暂停执行），调用 next() （再启动）方法时，如果传入了参数，那么这个参数会作为上一条执行的 yield 语句的返回值

#### `Async/Await`

`async/await` 是 `Generator` 的语法糖

```js
// Generator
run(function*() {
  const res1 = yield readFile(path.resolve(__dirname, '../data/a.json'), { encoding: 'utf8' });
  console.log(res1);
  const res2 = yield readFile(path.resolve(__dirname, '../data/b.json'), { encoding: 'utf8' });
  console.log(res2);
});

// async/await
const readFile = async ()=>{
  const res1 = await readFile(path.resolve(__dirname, '../data/a.json'), { encoding: 'utf8' });
  console.log(res1);
  const res2 = await readFile(path.resolve(__dirname, '../data/b.json'), { encoding: 'utf8' });
  console.log(res2);
  return 'done'；
}
const res = readFile();
```

`async/await` 的特点

- 当 `await` 后面是 `Promise` 对象时，才会异步执行，其它类型的数据会同步执行
- `async`函数内部`return`语句返回的值，会成为 then 方法回调函数的参数
- 执行 `const res = readFile();` 返回的仍然是个 Promise 对象，上面代码中的 `return 'done';` 会直接被下面 `then` 函数接收

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[前端进阶](https://muyiy.cn/)

[Async/Await 如何通过同步的方式实现异步](https://muyiy.cn/question/async/9.html)
