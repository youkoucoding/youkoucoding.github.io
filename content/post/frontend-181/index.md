---
title: '毎日のフロントエンド  181'
date: 2022-03-19T14:55:05+09:00
description: frontend 每日一练
categories:
  - HTML
  - CSS
  - JavaScript
---

## HTML

### `HTML`进度条

- `<progress>`HTML5 的一个新元素，表示定义一个进度条，用途很广泛，可以用在文件上传的进度显示，文件下载的进度显示，也可以作为一种 loading 的加载状态条使用。

  - max 属性表示进度条的进度最大值，如果有此值，必须是大于 0 的有效浮点数。max 的默认值是 1.
  - value 属性表示进度条完成的进度之，value 值的范围为 0-max 之间。如果没有设置 max 属性，那么 value 属性值的范围要在 0-1 之间。

- `<progress max='100'>` 加载状态条显示
- `<progress value='70' max='100'>` 正常显示

> caveat: 各个浏览器 UI 表现不统一

## CSS

### css 的预处理器和后处理器

- 预处理器：`Sass`, `LESS`, `Stylus`

  - 优点：语言级逻辑处理，动态特性，改善项目结构
  - 缺点：采用特殊语法，框架耦合度高，复杂度高

- 后处理器：`Rework`, `PostCSS`

  - 优点：使用 CSS 语法，容易进行模块化，贴近 CSS 的未来标准
  - 缺点：逻辑处理能力有限.

## JavaScript

### 密码强度校验的方法

```js
/**
 *方法说明
 *@createPassword 密码范围 {0-9，A-Z, a-z}
 *@param
 * num {num} 生成密码长度
 * b   {num} 密码生成类型，1: 数字， 2:数字+小写，3.数字+大小写
 *@return {str}
 **/
const createPassword = function (num, b) {
  let n = 0; // 循环次数
  let arr = new Array(); // 保存随机数
  // 随机取数0-2/0-1
  let Capitalization = () => Math.floor(Math.random() * b);
  let randomNumber = [
    () => Math.floor(Math.random() * (57 - 48 + 1) + 48),
    () => Math.floor(Math.random() * (122 - 97 + 1) + 97),
    () => Math.floor(Math.random() * (90 - 65 + 1) + 65),
  ]; // 去随机值

  for (let i = 0; i < num; i++) {
    arr.push(randomNumber[Capitalization()]());
  }
  return arr.map((e) => String.fromCharCode(e)).join('');
};

/**
 * 方法说明
 *@method passwordStrength 密码强度校验
 *@for 所属类名
 *@param 
 *        num{str} 密码
 *        len {num} 密码长度
 *@return {str} 返回密码等级
      str    ASCII
      0-9    48-57
      A-Z    65-90
      a-z    97-122
 */
let passwordStrength = function (num, len) {
  if (num.length < len) return '密码长度不够';
  let passwordLevel = ['弱', '普通', '较强', '强'];
  let arr = new Array(4); // 记录密码等级
  num
    .split('')
    .map((e) => e.charCodeAt())
    .forEach((e) => {
      if (e >= 48 && e <= 57) {
        arr[0] = 1;
      } else if (e >= 65 && e <= 90) {
        arr[1] = 1;
      } else if (e >= 91 && e <= 122) {
        arr[2] = 1;
      } else {
        arr[3] = 1;
      }
    });
  return passwordLevel[arr.reduce((a, b) => a + b) - 1];
};
```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[`<progress>`：进度指示元素 - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/progress)
