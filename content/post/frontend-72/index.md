---
title: '毎日のフロントエンド　 72'
date: 2021-11-27T12:06:32+09:00
description: frontend 每日一练
image: frontend-72-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第七十二日

## HTML

### **Question:** `video`和`audio`分别支持哪些格式

- `video`: MP4、WebM、Ogg
- `audio`: MP3、Wav、Ogg

## CSS

### **Question:** 怎么改变选中文本的文字颜色和背景色

- Pseudo-elements 伪元素

```css
::selection {
  background-color: #222;
  color: white;
}
```

## JavaScript

### **Question:** 重复字符串的 repeat 函数

```js
let str = 'abcd';
function repeat(str, n) {
  if (typeof str === 'string') {
    return new Array(n + 1).join(str);
  }
  return 'Type Error';
}
repeat(str, 3); //abcdabcdabcd
```

---

```js
const repeat = (str, n) => str.repeat(n);
```

---

```js
const repeatStr = (str, num) => {
  return Array(num + 1)
    .fill(str)
    .join('');
};
```

### **Question:** 写出执行结果，解释原因

```js
['1', '2', '3'].map(parseInt);
```

Result: `[1,NaN,NaN]`

1. `Array.prototype.map()`: The map() method **creates a new array**

   - `callbackFn` Three Parameters
     1. `element`:The current element being processed in the array.
     2. `index - Optional`: The index of the current element being processed in the array.
     3. `array - Optional`: The array map was called upon.
   - `thisArg - Optional`: Value to use as this when executing `callbackFn`.

2. `parseInt`: The `parseInt(string, radix)` function **parses a string** argument and returns an integer of the specified radix(基数).

   - `callbackFn`的第三个参数，忽略，因为，`parseInt`只接受两个参数。

     1. `string`

        - The value to parse. If this argument is not a string, then it is converted to one using the ToString abstract operation. Leading whitespace in this argument is ignored.

     2. `radix - Optional`

        - An integer between 2 and 36 that represents the radix (the base in mathematical numeral systems) of the string. Be careful—**this does not default to 10!** If the radix value is not of the `Number` type it will be coerced(迫使) to a `Number`.

     3. `return value`
        - An integer parsed from the given string.
        - or `NaN` when:
          1. the `radix` modulo `2**32` is **smaller than `2`** or bigger than `36`, or
          2. the first non-whitespace character cannot be converted to a number.

```js
parseInt('1', 0); // radix为0时，ECMAScript5将string作为十进制数字的字符串解析；
parseInt('2', 1); // radix为1时，解析结果为NaN
parseInt('3', 2); // radix在2—36之间时，如果string参数的第一个字符（除空白以外），不属于radix指定进制下的字符，解析结果为NaN。 parseInt("3", 2)执行时，由于"3"不属于二进制字符，解析结果为NaN。
```

> **在使用 `parseInt` 时，一定要指定一个 radix。**

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)

[parseInt() - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt)
