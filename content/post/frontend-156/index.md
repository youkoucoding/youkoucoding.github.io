---
title: '毎日のフロントエンド  156'
date: 2022-02-19T15:08:08+09:00
description: frontend 每日一练
categories:
  - CSS
  - Tips
---

## CSS

### `border:none`和`border:0px`区别

- 从`border: 10px;`过渡到`border: none;`，是不会有动画的
- 从`border: 10px;`过渡到`border: 0px;`，是可以有动画的

## Tips

### `JavaScript` 的内存管理

- 基本类型：这些类型在内存中会占据固定的内存空间，它们的值都保存在栈空间中，直接可以通过值来访问这些；
- 引用类型：由于引用类型值大小不固定（比如上面的对象可以添加属性等），栈内存中存放地址指向堆内存中的对象，是通过引用来访问的

> 栈内存中的基本类型，可以通过操作系统直接处理；而堆内存中的引用类型，正是由于可以经常变化，大小不固定，因此需要 JavaScript 的引擎通过垃圾回收机制来处理。

#### Chrome 内存回收机制

Chrome 的 JavaScript 引擎 V8 将堆内存分为两类 新生代的回收机制和老生代的回收机制

#### 内存泄漏与优化

- 内存泄漏的场景：
  - 过多的缓存未释放
  - 闭包太多未释放
  - 定时器或者回调太多未释放
  - 太多无效的 DOM 未释放
  - 全局变量太多未被发现

1. 减少不必要的全局变量，使用严格模式避免意外创建全局变量

```js
function foo() {
  // 全局变量=> window.bar
  this.bar = '默认this指向全局';
  // 没有声明变量，实际上是全局变量=>window.bar
  bar = '全局变量';
}
foo();
```

2. 在你使用完数据后，及时解除引用（闭包中的变量，DOM 引用，定时器清除）

```js
var someResource = getData();
setInterval(function() {
    var node = document.getElementById('Node');
    if(node) {
        node.innerHTML = JSON.stringify(someResource));
        // 定时器也没有清除，可以清除掉
    }
    // node、someResource 存储了大量数据，无法回收
}, 1000);
```

3. 组织好代码逻辑，避免死循环等造成浏览器卡顿、崩溃的问题

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)
