---
title: '毎日のフロントエンド　62'
date: 2021-11-17T10:03:17+09:00
description: frontend 每日一练
image: frontend-62-cover.jpg
categories:
  - HTML
  - Javascript
---

# 第六十二日

## HTML

### **Question:** HTML5 的拖放 API

- 图片默认自带拖拽功能，非图片元素设置 `draggable` 属性为 `true` 即可拖拽。

- 被拖拽元素的事件：

  1. `ondragstart` 拖拽的一瞬间触发
  2. `ondrag` 拖拽期间连续触发
  3. `ondragend` 拖拽结束触发

- 目标元素事件（将拖拽元素释放的地方）：

  1. `ondragenter` 进入目标元素触发（鼠标光标进入）
  2. `ondragover` 进入离开目标元素连续触发
  3. `ondragleave` 离开目标元素触发
  4. `ondrop` 在目标元素上释放鼠标触发

默认状态下，一个元素不能放在另一个元素上面，需要在 `ondragover` 上阻止默认事件。

## JavaScript

### **Question:** 写出执行结果,并解释原因 一

```js
var fullname = 'a';
var obj = {
  fullname: 'b',
  prop: {
    fullname: 'c',
    getFullname: function () {
      return this.fullname;
    },
  },
};

console.log(obj.prop.getFullname()); // c
var test = obj.prop.getFullname;
console.log(test()); // a
```

Explain:

- `this` 指向的是函数的执行环境，`this` 取决于其被谁调用了，而不是被谁定义了
- 对第一个 `console.log()`语句而言，`getFullName()`是作为 `obj.prop` 对象的一个方法被调用的，因此此时的执行环境应该是这个对象。
- 另一方面，但 `getFullName()`被分配给 `test` 变量时，此时的执行环境变成全局对象（`window`），原因是 test 是在全局作用域中定义的。因此，此时的 `this` 指向的是全局作用域的 `fullname` 变量，即 `a` (`var` 声明)

### **Question:** 写出执行结果,并解释原因 二

```js
var company = {
  address: 'toki',
};
var abc = Object.create(company);
delete abc.address;
console.log(abc.address); // toki
```

Explain:

这里的 `abc` 通过 prototype 继承了 company 的 address。abc 自己并没有 address 属性。所以 `delete` 操作符无效。

- note one：

  - `delete` 操作符用于删除对象的某个属性；如果没有指向这个属性的引用，那它最终会被释放。
  - `delete` 的操作对象应是**某个属性的引用**
  - 返回值： **对于所有情况都是 `true`**，除非属性是一个自身的 _不可配置的属性_，在这种情况下，非严格模式返回 false。:如果你试图删除的属性不存在，那么`delete` 将不会起任何作用，但仍会返回`true`
  - `delete` 操作符**与直接释放内存无关**。

- note two:

  - `delete` 在删除一个不可配置的属性时在严格模式和非严格模式下的区别:

    1. 在严格模式中，如果属性是一个不可配置（`non-configurable）`属性，删除时会抛出异常;
    2. 非严格模式下返回 false

  - `delete` 能删除隐式声明的全局变量：这个全局变量其实是`global`对象(`window`)的属性

  - `delete` 可以删除的:

    1. 可配置对象的属性
    2. 隐式声明的全局变量
    3. 用户定义的属性
    4. 在 ES6 中，通过 `const` 或 `let` 声明指定的 `temporal dead zone` (TDZ) , `delete` 操作符也会起作用

  - `delete`不能删除的:

    1. 显式声明的全局变量
    2. 内置对象的内置属性
    3. 一个对象从原型继承而来的属性(如，本题)

  - `delete` 删除数组元素:

    1. 删除一个数组元素时，数组的 `length` 属性并不会变小，数组元素变成`undefined`
    2. 用 `delete` 操作符删除一个数组元素时，被删除的元素已经完全不属于该数组
    3. 如果想让一个数组元素的值变为 `undefined` 而不是删除它，可以使用 `undefined` 给其赋值而不是使用 `delete` 操作符。此时数组元素是在数组中的

Example:

```js
var output = (function(x){
    delete x;
    return x;
})(0);
console.log(output); // 0
// delete 操作符是将object的属性删去的操作。但是这里的 x 是并不是对象的属性， delete 操作符并不能作用
```

---

```js
var x = 1;
var output = (function(){
    delete x;
    return x;
})();
console.log(output); // 1  同上
```

---

```js
var x = { foo: 1 };
var output = (function () {
  delete x.foo;
  return x.foo;
})();
console.log(output); // undefined
// x虽然是全局变量，但是它是一个object。delete作用在x.foo上，成功的将x.foo删去。所以返回 undefined
```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)

[SortableJS/Sortable: Reorderable drag-and-drop lists for modern browsers and touch devices. No jQuery or framework required.](https://github.com/SortableJS/Sortable)

[delete 操作符 - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/delete)
