---
title: '毎日のフロントエンド　 78'
date: 2021-12-03T11:42:03+09:00
description: frontend 每日一练
image: frontend-78-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第七十八日

## HTML

### **Question:** `HTML5`中的`datalist`标签

`<datalist>`: The HTML data list element.

HTML `<datalist>`元素包含了一组`<option>`元素，这些元素表示其它表单控件可选值.

用于在用户输入时给出一批建议数据 如果需要用到`datalist` 请给对应的`input`的`list`属性和`datalist`的 ID 属性设置上一样的属性值 `datalist`给出的选项用`<option>`包裹 选项值用 option 的`value`属性给出。

`datalist`支持全局属性和事件属性。

```html
<input list="ice-cream-flavors" id="ice-cream-choice" name="ice-cream-choice" />
<!-- list 等于 id 标签中的id要与标签中的list相对应 -->
<datalist id="ice-cream-flavors">
  <option value="Chocolate"></option>
  <option value="Coconut"></option>
  <option value="Mint"></option>
  <option value="Strawberry"></option>
  <option value="Vanilla"></option>
</datalist>
```

## CSS

### **Question:** 遇到`overflow: scroll`不能平滑滚动怎么解决(移动端)

```css
-webkit-overflow-scrolling: touch;
```

## JavaScript

### **Question:** 数组和对象的迭代方法分别有哪些

#### `Array`

1. `forEach`[^1]: 无返回值，不可中断
2. `map`[^2]: 不会修改原数组，返回一个新的数组。如果没写 `return` 就会返回 `undefined`
3. `filter`[^3]: 根据输入的条件将返回值为 `true` 的项重新组成数组返回出去
4. `some((item, index))`[^4] / `every((item, index))`[^5]：都是对输入的条件进行判断，`some` 只要有一个满足就返回 `true`; `every` 是需要所有项都满足才返回 `true`
5. `reduce((prev,cur, index))`[^6] / `reduceRight((prev, cur, index))` [^7]：允许我们对数组前一项和当前项进行操作，自定义返回值类型
6. `for of`[^8]

#### `Object`

- `Object.keys(obj)`[^9]
- `Object.values`[^10]
- `Object.getOwnPropertyNames`[^11]
- `Object.entries`[^12]
- `for in`[^13]

[^1]: [Array.prototype.forEach() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)
[^2]: [Array.prototype.map() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/map)
[^3]: [Array.prototype.filter() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)
[^4]: [Array.prototype.some() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/some)
[^5]: [Array.prototype.every() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/every)
[^6]: [Array.prototype.reduce() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)
[^7]: [Array.prototype.reduceRight() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/reduceRight)
[^8]: [for...of - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...of)
[^9]: [Object.keys() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/keys)
[^10]: [Object.values() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/values)
[^11]: [Object.getOwnPropertyNames() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyNames)
[^12]: [Object.entries() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/entries)
[^13]: [for...in - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...in)

### **Question:** 写出执行结果,并解释原因

```js
var arr = [0, 1];
arr[5] = 5;
newArr = arr.filter(function (x) {
  return x === undefined;
});
console.log(newArr.length);
```

Result: 0

- `filter` 为数组中的每个元素调用一次 `callback` 函数，并利用所有使 `callback` 返回 `true` 或等价于 `true` 的值的元素创建一个新数组。
- `callback` 只会在已经赋值的索引上被调用， 跳过从未分配过值的索引， 对于那些已经被删除或者从未被赋值的索引不会被调用。那些没有通过 callback 测试的元素会被跳过，不会被包含在新数组中。
- 也就是说 从 2-4 都是没有初始化的元素!, 这些索引并不存在与数组中. 在 array 的函数调用的时候是会跳过。

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)

[datalist - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/datalist)
