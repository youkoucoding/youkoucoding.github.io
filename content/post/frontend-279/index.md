---
title: '毎日のフロントエンド 279~285'
date: 2022-05-23T20:31:14+09:00
description: frontend 每日一练
categories:
  - HTML
  - Tips
---

## HTML

### html 缺少了结束标签，如何检测预防

- W3C 验证服务：[The W3C Markup Validation Service](https://validator.w3.org/)

## CSS

### `baseline`

- baseline 是西文字体里面的一种定位
  - `vertical-align:baseline` 是指行内元素里的文字，在垂直方向上，按字体的基线排列，
  - `axec` 这些字母的底部就是`baseline`，然后 lh 的 baseline 也一样
  - `g`的`baseline`则在于中间，就是西文字体如何在一条看不见的线上排练形成整齐的视觉效果

## JavaScript

### `Window.getSelection`

- 返回一个 Selection 对象，表示用户选择的文本范围或光标的当前位置。

## Tips

### V8 引擎特性带来的的 JS 性能变化

- `try catch` 对性能的影响忽略不计
  - [Callback Promise Generator Async-Await 和异常处理的演进 · Issue #14 · ascoders/blog](https://github.com/ascoders/blog/issues/14)
- 解决 delete 性能问题
- arguments 转数组性能已不是问题
  - node8.3 版本及以上，该使用拓展运算符获取参数，不但没有性能问题，可读性也大大提高，结合 ts 时也能得到类型支持
- `bind` 对性能影响可以忽略
  - 在 react 中副作用仍需警惕, 推荐使用箭头函数书写**成员函数**
- 函数调用对性能影响越来越小
  - 对函数调用优化的越来越好，不需要过于担心注释与空白、函数间调用对性能的影响
- 32 64 位数字计算性能
  - node8 对超长数字计算性能还是较低，大概是 32 位数字性能的 2/3，所以**尽量用字符串处理大数**
- 遍历 object
  - 基本用法有 `for in` `Object.keys` `Object.values`. 在 node8 中，`for in` 将变得更慢，但仍然比其他两种方法快，所以，尽早取消不必要的优化
- 创建对象速度在 node8 得到极大提升

## Reference

- [fe-interview/history.md](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

- [Window.getSelection - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/getSelection)

- [Selection - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Selection)

- [Callback Promise Generator Async-Await 和异常处理的演进 · Issue #14 · ascoders/blog](https://github.com/ascoders/blog/issues/14)
