---
title: '毎日のフロントエンド 307~310'
date: 2022-06-01T21:55:32+09:00
description: frontend 每日一练
categories:
  - CSS
  - JavaScript
---

## CSS

### 去除 img 之间存在的间隔

1. `display：block`
2. `img {vertical-align:top、middle、bottom;}`
3. 父元素`font-size:0;`
4. `img {float:left;}`
5. 取消图片标签和其父对象的最后一个结束标签之间的空格
6. 如果父对象的宽、高固定，图片大小随父对像而定，那么可以设置： `overflow:hidden`

### 内联元素设置宽和高

1. 正常情况下不能设置宽高(可替换内容元素除外例如 img)
2. 元素被浮动，绝对定位，固定定位后，可以设置宽高
3. display:block,display:inline-block 等后可以设置宽高
4. 内联元素的宽高是由其内容决定的，并且在一行显示（可以换行），呈现包裹性,因此设置宽高无效

## JavaScript

### `parseInt()`和`Number()`区别

```js
// 当字符串是由数字组成的时候 他们转换的数字一样的没有差别
let numStr = '123';
console.log(parseInt(numStr)); //123
console.log(Number(numStr)); //123

// 当字符串是由字母组成的时候
let numStr = 'abc';
console.log(parseInt(numStr)); //NaN
console.log(Number(numStr)); //NaN

// 当字符串是由数字和字母组成的时候
let numStr = '123a';
console.log(parseInt(numStr)); //123
console.log(Number(numStr)); //NaN

// 当字符串是由0和数字
let numStr = '0123';
console.log(parseInt(numStr)); //123
console.log(Number(numStr)); //123

// **当字符串包含小数点**
let numStr = '123.456';
console.log(parseInt(numStr)); //123
console.log(Number(numStr)); //123.456

// **当字符串为null时**
let numStr = null;
console.log(parseInt(numStr)); //NaN
console.log(Number(numStr)); //0

// **当字符串为''(空)时**
let numStr = '';
console.log(parseInt(numStr)); //NaN
console.log(Number(numStr)); //0
```

1. parsetInt 返回整数或者 NaN，Number 返回数字(整数和小数)或者 NaN
2. parsetInt(string,radix) string 是数字或者字符串，radix 为 2~32 为转化后的的基数，当为 undefined 或者 0 时取默认值为 10，不在 2\~32 将返回 NaN
3. parsetInt 的 string 以其它字符结尾时，例如 parseInt('11s')得到 11
4. Number 的参数为 object，不是 obejct 的将包装成 object，但是转化'11s'将得到 NaN，Number([])===0 ;Number(false)===1
5. 总结: parseInt 更像正则转化，尽可能的将字符串转为整数，Number 是将对象转化为数值型数据和隐式转化为数字一样

## Reference

- [fe-interview/history.md](https://github.com/haizlin/fe-interview/blob/master/category/history.md)
