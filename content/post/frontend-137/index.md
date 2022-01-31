---
title: '毎日のフロントエンド  137'
date: 2022-01-31T12:21:29+09:00
description: frontend 每日一练
image: frontend-137-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第一百三十七日

## CSS

### **Question:** z-index 不起作用时的原因是什么

满足下面任一条件会形成层叠上下文:

- 文档根元素（`<html>`）；
- `position` 值为 `absolute`（绝对定位）或 `relative`（相对定位）**且** `z-index` 值不为 `auto` 的元素；
- `position` 值为 `fixed`（固定定位）或 `sticky`（粘滞定位）的元素（沾滞定位适配所有移动设备上的浏览器，但老浏览器不支持）；
- `flex` 容器的子元素，**且** `z-index` 值不为 `auto`；
- `grid`容器的子元素，**且** `z-index` 值不为 `auto`；
- `opacity` 属性值小于 1 的元素；
- `mix-blend-mode` 属性值不为 normal 的元素；
- 以下任意属性值不为 `none` 的元素：
  - transform
  - filter
  - perspective
  - clip-path
  - mask / mask-image / mask-border
- `isolation 属性值为 `isolate` 的元素；
- `-webkit-overflow-scrolling` 属性值为 `touch` 的元素；
- `will-change` 值设定了任一属性而该属性在 `non-initial` 值时会创建层叠上下文的元素；
- `contain` 属性值为 `layout`、`paint` 或包含它们其中之一的合成值（比如 contain: strict、contain: content）的元素。

---

- 层叠上下文可以包含在其他层叠上下文中，并且一起创建一个层叠上下文的层级。
- 每个层叠上下文都完全独立于它的兄弟元素：当处理层叠时只考虑子元素。
- 每个层叠上下文都是自包含的：当一个元素的内容发生层叠后，该元素将被作为整体在父级层叠上下文中按顺序进行层叠。

## JavaScript

### **Question:** 用 js 实现页面局部打印和预览原理

```html
<h1>不打印1</h1>
<!--startprint-->
<div class="content">要打印的内容</div>
<!--endprint-->
<h1>这块内容不需要打印2</h1>
<button onclick="doPrint()">打印按钮</button>
```

```js
function doPrint() {
  bdhtml = window.document.body.innerHTML;
  sprnstr = '<!--startprint-->';
  eprnstr = '<!--endprint-->';
  prnhtml = bdhtml.substr(bdhtml.indexOf(sprnstr) + 17);
  prnhtml = prnhtml.substring(0, prnhtml.indexOf(eprnstr));
  window.document.body.innerHTML = prnhtml;
  window.print();
}
```

## Tips

### JavaScript 数据类型强制类型转换，隐式转换

#### 强制类型转换

- `Number()`
- `parseInt()`
- `parseFloat()`
- `Boolean()`
- `String()`
- `toString()`

Notes:

```js
Number(null); // 0
Number(''); // 0
parseInt(''); // NaN
```

- `Number()`

  - `true` `false` 转换为 `1` `0`
  - 数字返回自身
  - `null` 返回 `0`
  - `undefined` 返回 `NaN`
  - `string`
    - 如果字符串中只只包含数字，则转换为十进制数
    - 如果字符串中包含有效浮点格式，则转为相应的浮点数值
    - 空字符串，转化为 `0`
    - 若不是以上情况，均返回 `NaN`
  - `Symbol` 抛出错误
  - 对象， 且包含`[Symbol.toPrimitive]`, 调用此方法，否则调用对象的 `valueOf()`方法
    - 将以上方法的返回值，按照上述规则进行类型转换，并返回最后结果
      - 如果转换的结果是 `NaN`，则再继续调用对象的`toString()`方法后，再次根据上述规则进行转换，并返回最后的结果。

- `Boolean()`

  - 除了 `undefined`,`null`,`false`,`0`,`+0`,`-0`, `NaN`, 全部结果都为`true`

#### 隐式类型转换

- 当不同数据类型的数据，遇到以下操作符时， 都会发生隐式了行转换:
  - 逻辑运算符(`&&`, `||`...)，运算符(`+`, `-`...)， 关系操作符(`>`, `<`...)，相等运算符(`==`)，`if` `while`

1. `==`的隐式转换规则

   - 如果其中一个操作数是 `null`或`undefined`， 那么另一个操作数必须为 `null`或`undefined`，结果才是`true`,其余均返回`false`
   - 如果有`Symbol`类型，则返回`false`
   - 两个操作数都为， `string`和`number`， 则转换为`number`后 进行比较
   - 操作数有`boolean`， 将`boolean` 转换为 `number`
   - 操作数有一个`object`， 且，另外一个操作数为`number` or `string` or `symbol`，会把 `object` 转化为基本数据类型(调用`valueOf()` `toString()`) 再进行判断

2. `+`隐式类型转换

   - 两侧都是字符串时，直接进行拼接
   - 如果**其中一个是字符串**，另一个是 `undefined` `null` `boolean` 则对其调用 `toString()` 然后进行字符串拼接。
     - 若是纯对象，数组，正则等，则默认调用对象的转换方法（调用方式有优先级）， 然后字符串拼接
   - 如果**其中一个是数字**，另一个是 `undefined` `null` `boolean` or **数字**， 则会将其转换为数字，进行加法运算
     - 若是对象类型，则参考上一条规则
   - 数字 `+` 字符串， 则进行字符串拼接

3. `object` 转换规则

   1. 如果有`[Symbol.toPrimitive]`， 则优先调用此方法，并返回
   2. 调用`valueOf()`, 若结果为基本数据类型， 则返回
   3. 调用`toString()`, 若结果为基本数据类型， 则返回
   4. 若都没有返回基本数据类型， 会抛出错误

   example：

   ```js
   10 + {};
   // '10[object Object]'
   //{} 会默认调用 valueOf 结果是{}, 不是基础数据类型，继续转换=》调用 toString() 返回'[object Object]' 然后字符串拼接。
   ```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[层叠上下文 - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Positioning/Understanding_z_index/The_stacking_context)

[Number - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number)
