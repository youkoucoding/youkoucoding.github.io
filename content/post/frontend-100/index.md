---
title: '毎日のフロントエンド  100'
date: 2021-12-25T20:03:03+09:00
description: frontend 每日一练
image: frontend-100-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第一百日

## HTML

### **Question:** `HTML5`为输入框添加语音输入的功能呢

原理：在人说话时接收语音，然后放到谷歌后台，谷歌自己写的语音识别系统，再返回数据
`<input type=”text” speech x-webkit-speech />`

## CSS

### **Question:** 如何让大小不同的图片等比缩放不变形显示在固定大小的`div`里

- 图片等比缩放 `img{ object-fit: cover/contain; width:100%}`
- div 宽高比例固定，跟随屏幕变化而变化，利用 padding 垂直方向的属性来实现

- `object-fit`: 属性指定可替换元素的内容应该如何适应到其使用的高度和宽度确定的框。
  - 典型的可替换元素有：
    - `<iframe>`
    - `<video>`
    - `<embed>`
    - `<img>`
  - `contain`: 被替换的内容将被缩放，以在填充元素的内容框时保持其宽高比。 整个对象在填充盒子的同时保留其长宽比，因此如果宽高比与框的宽高比不匹配，该对象将被添加“黑边”。
  - `cover`: 被替换的内容在保持其宽高比的同时填充元素的整个内容框。如果对象的宽高比与内容框不相匹配，该对象将被剪裁以适应内容框。
  - `fill`: 被替换的内容正好填充元素的内容框。整个对象将完全填充此框。如果对象的宽高比与内容框不相匹配，那么该对象将被拉伸以适应内容框。
  - `none`: 被替换的内容将保持其原有的尺寸。
  - `scale-down`: 内容的尺寸与 none 或 contain 中的一个相同，取决于它们两个之间谁得到的对象尺寸会更小一些。

## JavaScript

### **Question:** 为什么 for 循环嵌套顺序会影响性能？

```js
var t1 = new Date().getTime();

for (let i = 0; i < 100; i++) {
  for (let j = 0; j < 1000; j++) {
    for (let k = 0; k < 10000; k++) {}
  }
}
var t2 = new Date().getTime();
console.log('first time', t2 - t1);

for (let i = 0; i < 10000; i++) {
  for (let j = 0; j < 1000; j++) {
    for (let k = 0; k < 100; k++) {}
  }
}
var t3 = new Date().getTime();
console.log('two time', t3 - t2);
```

- 第一个`for`循环，`i`会循环 100 次，判断`i<100`, **100** 次, j 会循环`100 * 1000`次，判断`j<100`, **`100 * 1000`** 次, k 会循环`100 * 1000 * 10000`次，判断`k<100`, **`100 * 1000 * 10000`**

- 第二个`for`循环，`i`会循环 10000 次，判断 i`<100` **10000** 次, j 会循环`10000 * 1000`次，判断`j<100`, **`10000 * 1000`** 次, k 会循环`100 * 1000 * 10000`次， 判断`k<100`, **`100 * 1000 * 10000`** 次

虽然判断`k<100`的次数都是一样的 但是前面两种判断就不一样了，由此可以看见时间长短。外层越大，越影响性能。

### **Queston:** 代码运行结果

```js
1 + '1';
2 * '2';
[(1, 2)] + [2, 1];
'a' + +'b';
```

- `1 + "1"`
  - `"11"`
  - 加操作符：如果只有一个操作数是字符串，则将另一个操作数转换为字符串，然后再将两个字符串拼接起来
- `2 * '2'`
  - `4`
  - 乘操作符：如果有一个操作数不是数值，则在后台调用 `Number()`将其转换为数值
- `[(1, 2)] + [2, 1];`
  - js 中所有对象基本都是先调用`valueOf`方法，如果不是数值，再调用`toString`方法。
  - 两个数组对象的 `toString` 方法相加：`"1,22,1"`
- `'a' + + 'b'`
  - 后一个`+`将作为一元操作符，如果操作数是字符串，将调用`Number`方法将该操作数转为数值，如果操作数无法转为数值，则为`NaN`。
  - `"aNaN"`

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[前端进阶](https://muyiy.cn/)

[object-fit - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/object-fit)
