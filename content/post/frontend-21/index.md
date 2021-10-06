---
title: '毎日のフロントエンド　21'
date: 2021-10-06T12:26:28+09:00
description: frontend 每日一练
image: frontend-21-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第二十一日

## HTMl

### **#Question:** 谈谈对`input`元素中`readonly`和`disabled`属性的理解

相同点：

- 都会使文本框变成只读，不可编辑

不同点：

1. `disabled` 属性在将 `input` 文本框变成只读不可编辑的同时，还会使文本框变灰, 不允许填写和使用,但是`readonly`不会
2. `disabled` 属性修饰后的文本框内容，在不可编辑的同时，通过 **js 也是获取不到的**。但是用 readonly 修饰后的文本框内容，是可以通过 js 获取到的，也就只是简单的不可编辑而已
3. `disabled` 属性对 `input` 文本框，单选 `radio`, 多选 `checkbox` 都适用，但是 `readonly` 就不适用，用它修饰后的单选以及多选按钮仍然是可以编辑状态的。(`readonly`只针对`input`和`textarea`有效，而 disabled 对于所有的表单元素都有效)
4. `readonly` 直译为 “只读”，一般用于只允许用户填写一次的信息，提交过一次之后，就不允许再次修改了
5. `disabled` 的数据是不会被获取和上传，`readonly` 的数据会被获取和上传

Summary：

1. `readonly`：不可编辑、可复制、可选择、可以接收焦点但不能被修改，后台会接收到传值
2. `disabled`：不可编辑、不可复制、不可选择、不能接收焦点，后台也不会接收到传值

## CSS

### **#Question:** 说说对`line-height`是如何理解的？

`line-height`

The `line-height` CSS property sets the height of a line box. It's commonly used to set the distance between lines of text. On block-level elements, it specifies the **minimum height of line boxes** within the element. On ono-replaced inline elements, it specifies the height that is used to calculate line box height[^1].

[^1]: [line-height - CSS: Cascading Style Sheets | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/line-height#values)

`line-height` 在日常用的最多的是让单行文字垂直居中（其实不需要设置 height，一个 line-height 即可）。

`line-height - font-size` 为行距，一般会近似平分到文字的上下两边，使文字看上去垂直居中。如果需要多行文字的垂直居中，还需要加上 `vertical-align: middle;`

`line-height` 可以**不设置单位**，表示 `font-size` 的倍数

对于非替换元素的纯内联元素，其高度是由 `line-height` 所决定的

## JavaScript

### **#Question:** 写一个方法验证是否为中文[^2]

```js
//使用的Unicode 编码 4e00 和 9fa5 分别表示第一个汉字和最后一个汉字的编码
function isChinese(str) {
  const reg = /^[\u4e00-\u9fa5]+$/;
  return reg.test(str);
}
```

[^2]: [中文字符对应编写正则](https://github.com/haizlin/fe-interview/issues/72#issuecomment-544332674)

## Reference

[CSS 深入理解之 line-height](https://juejin.cn/post/6844903721025929223)
