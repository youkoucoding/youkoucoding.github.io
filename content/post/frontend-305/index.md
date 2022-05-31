---
title: '毎日のフロントエンド 305~306'
date: 2022-05-31T21:18:21+09:00
description: frontend 每日一练
categories:
  - CSS
  - JavaScript
---

## CSS

### 相对定位、绝对定位、固定定位

- `position` 属性指定了元素的定位类型
- `position` 属性的五个值：

  - `static`（默认值）
  - `relative`（相对定位）
  - `fixed`（固定定位）
  - `absolute`（绝对定位）
  - `sticky`（粘性定位）

- `relative` 相对定位：相对自身元素的原来进行定位

  - 移动相对定位元素，但它原本所占的空间不会改变
  - 相对定位元素经常被用来作为绝对定位元素的容器块
  - 用途:
    - 第一个，为微调元素的位置
    - 第二个，做绝对定位的参考(父 relative, 子 absolute)

- `absolute` 绝对定位：绝对定位的元素的位置相对于最近的已定位父元素，如果元素没有已定位的父元素，那么它的位置相对于`<html>`（初始包含块）

  - `absolute` 定位使元素的位置与文档流无关，因此不占据空间
  - `absolute` 定位的元素和其他元素重叠

- `fixed` 固定定位：元素的位置相对于浏览器窗口是固定位置

  - 即使窗口是滚动的它也不会移动
  - Fixed 定位使元素的位置与文档流无关，因此不占据空间
  - Fixed 定位的元素和其他元素重叠
  - 用途:
    - 固定到浏览器窗口固定位置的元素
    - 跟随导航
    - 回到顶部

- `sticky` 粘性定位：粘性定位的元素是依赖于用户的滚动，在 position:relative 与 position:fixed 定位之间切换
  - 行为就像 position:relative; 而当页面滚动超出目标区域时，它的表现就像 position:fixed;，它会固定在目标位置。
  - 元素定位表现为在跨越特定阈值前为相对定位，之后为固定定位。
  - 这个特定阈值指的是 top, right, bottom 或 left 之一，换言之，指定 top, right, bottom 或 left 四个阈值其中之一，才可使粘性定位生效。否则其行为与相对定位相同
  - 用途：页面吸顶效果

## JavaScript

### 表达式和语句

- 表达式(`Expression`)是语句(`Statement`)的子集
- 表达式一定会返回一个值，而语句不会

  - 比如定义变量、返回语句都属于语句，而逻辑判断、方法调用、赋值都属于表达式

- 表达式：

```js
const foo = 'foo';

return foo;
```

- 语句

```js
foo = 'foo';
foo > bar;
foo();
```

- 在仅支持表达式的位置写语句，则会报错

```js
for( 语句, 表达式, 表达式 ) {
语句
}

for(let i = 0; let j = 0; i++) {  // Uncaught SyntaxError: Unexpected identifier
  console.log(i);
}

//======

() => 表达式

let foo = () => return 'foo';  // Uncaught SyntaxError: Unexpected token 'return'
```

## Reference

- [fe-interview/history.md](https://github.com/haizlin/fe-interview/blob/master/category/history.md)
