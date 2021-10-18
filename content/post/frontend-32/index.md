---
title: '毎日のフロントエンド　32'
date: 2021-10-18T17:44:05+09:00
description: frontend 每日一练
image: frontend-31-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第三十二日

## HTML

### **Question:** 说说对 `HTML` 元素的显示优先级的理解

Day20 类似。

帧元素（`frameset`) 优先级最高 `>>>` 表单元素 `>` 非表单元素，即 `input type="radio"` 之类的表单控件 `>` 普通的如 `a,div` 等元素。

从有窗口和无窗口元素来分，有窗口元素 > 无窗口元素。有窗口元素如 `Select` 元素、`Object` 元素。

`z-index` 属性也可以改变显示优先级，但只对同种类型的元素才有效。

## CSS

### **Question:** `line-height`三种赋值方式有何区别

`line-height` 可以有带单位及不带单位的写法

```css
div {
  line-height: 24px;
  line-height: 1.5;
  line-height: 1.5em;
  line-height: 150%;
}
```

由于 `line-height` 有继承性，直接在某个元素上使用 line-height，以下这三种写法是没有区别的，比如给所有的 p 标签添加行高属性：

```css
p {
  line-height: 1.5em;
}
p {
  line-height: 1.5;
}
p {
  line-height: 150%;
}
```

三种方式的区别在于，给父元素设置行高的时候子元素的继承方式:

- 带有单位的 `line-height` 会被计算成 `px` 后继承。子元素的 `line-height` `=` 父元素的 `line-height * font-size` （如果是 px 了就直接继承）

- 而不带单位的 `line-height` 被继承的是倍数，子元素的 `line-height` `=` 子元素的 `font-size *` 继承的**倍数**

## JavaScript

### **Question:** 造成内存泄漏的操作有哪些

1. 闭包
2. 无效的全局变量
3. `addEventListener` didn't remove 副作用未清除
4. `setInterval` didn't clear
5. 还有一种是递归的时候不用尾调用优化，如果层级比较深的话会造成内存消耗激增，甚至程序崩溃，但是只要递归完成了，这些内存会被释放

## Reference

[line-height 3 种设置方式的区别 - 掘金](https://juejin.cn/post/6844903556739235848)
