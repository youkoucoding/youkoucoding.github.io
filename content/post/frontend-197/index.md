---
title: '毎日のフロントエンド 197'
date: 2022-04-04T16:21:24+09:00
description: frontend 每日一练
categories:
  - HTML
  - Tips
---

## HTML

### 举例说明`<a>`作用

- 打开链接
  - 当前页面打开 `<a href="https://www.github.com">test</a>`
  - 新标签页打开 `<a target="_blank" href="https://www.github.com">test</a>`
- 文件下载
  - `<a href="https://codeload.github.com/vkboo/vue-svg-board/zip/master" download="vue-board-svg-name">vue-board-svg</a>`
- 利用 URL Scheme 打开 app
  - `<a href="imeituan://xxx.xxx.xxx"></a>`
- mailto & telto
  - `<a href="mailto:xxx@site.com"></a>`
- 锚点
  - `<a href="#content">go to content</a>`
  - `<section id="content"></section>`

## CSS

### 段落的首行缩进

- `text-indent`属性
  - 能定义一个**块元素**首行文本内容之前的缩进量
  - 长度值：px em rem
  - 百分比：取决于包含块的 width
  - 关键字：
    - `each-line`：文本缩进会影响第一行，以及使用强制断行后的第一行；
    - `hanging`：该值会对所有的行进行反转缩进：除了第一行之外的所有的行都会被缩进，看起来就像第一行设置了一个负的缩进值。
  - 全局值：`inherit` `initial` `unset`

```css
/* <length> 长度值 */
text-indent: 3mm;
text-indent: 40px;

/* <percentage>百分比值取决于其包含块（containing block）的宽度*/
text-indent: 15%;

/* 关键字 */
text-indent: 5em each-line;
text-indent: 5em hanging;
text-indent: 5em hanging each-line;

/* 全局值 */
text-indent: inherit;
text-indent: initial;
text-indent: unset;
```

## JavaScript

#### 找出一段话里面出现频率最多的单词

```js
const mostOftenWord = (para) =>
  para
    .split(/\W+/)
    .reduce(
      (acc, word) => (
        (word = word.toLowerCase()),
        (acc[word] = (acc[word] || 0) + 1),
        acc.$.time < acc[word] ? ((acc.$.word = word), (acc.$.time = acc[word]), acc) : acc
      ),
      { $: { word: '', time: -1 } }
    ).$;

// test
mostOftenWord('I have a pen, I have an apple, Uh! apple pen. Pen pineapple apple pen pen.');
// {word: 'pen', time: 4}
```

tips:

1. `(',' + para).split(/\W+/)`
   - `\W` 元字符用于查找**非单词字符** `\w` 元字符用于查找**单词字符**
2. arrow function
   - `(param1, param2, …, paramN) => expression` //相当于：`(param1, param2, …, paramN) =>{ return expression; }`
   - `,` comma operator 逗号操作符 对它的每个操作数求值（从左到右），并返回最后一个操作数的值
     - 上述回调函数，利用逗号运算符，依次对三个表达式进行计算，并计算返回最后一个表达式的值

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[String.prototype.split() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/split)

[逗号操作符 - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Comma_Operator)
