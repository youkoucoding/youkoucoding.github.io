---
title: '毎日のフロントエンド　18'
date: 2021-10-02T23:10:17+09:00
description: frontend 每日一练
image: frontend-18-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第十八日

## HTML

### **#Question:** 怎样在页面上实现一个圆形的可点击区域？

1. 用 `canvas` 画布，弧线画圆，然后计算鼠标的坐标是否落在圆内
2. 利用 `SVG` 作出圆形，然后添加点击事件
3. 用一个 `div`,给 `div` 添加圆角属性`50%`，在 div 上添加点击事件
4. 利用 `<map>` 和 `<area>` 标签设置圆形点击区域<cite>`<area> & <map>` [^1]</cite>
   [^1]: [HTML area map 标签及在实际开发中的应用](https://www.zhangxinxu.com/wordpress/2017/05/html-area-map/)

```html
<!-- svg 圆 -->
<svg
  width="100%"
  height="100%"
  version="1.1"
  xmlns="http://www.w3.org/2000/svg"
>
  <circle
    cx="100"
    cy="50"
    r="40"
    stroke="black"
    stroke-width="2"
    fill="red"
    onclick="alert(3)"
  />
</svg>
```

## CSS

### **#Question:** 什么是 `FOUC`？ 如何避免 `FOUC` 的？

**FOUC 浏览器样式闪烁**

`FOUC` 即 `Flash of Unstyled Content`，是指页面一开始以样式 A（或无样式）的渲染，突然变成样式 B。原因是样式表的晚于 HTML 加载导致页面重新进行绘制。

产生原因

- 通过 @import 方式导入CSS
- `style` 标签在 `body` 中或底部
- 有几个样式表，放在html结构的不同位置

解决方案： **把 `link` 标签将样式放在 `head` 中**

## JavaScript

### **#Question:** 你理解的`use strict`, 是什么?使用它有什么优缺点？

由于历史原因 `JavaScript` 在错误提示方面做的并不完善。比如允许定义未声明的变量、不允许使用八进制数字、不允许函数参数重名、不允许删除不可删除的属性。



Pros:

- 消除 Javascript 语法的一些不合理、不严谨之处，减少一些怪异行为
- 消除代码运行的一些不安全之处，保证代码运行的安全
- 提高编译器效率，增加运行速度
- 为未来新版本的 Javascript 做好铺垫 <cite>`use strict` [^2]</cite>

Strict mode makes several changes to normal JavaScript semantics:

- Eliminates some JavaScript silent errors by changing them to throw errors.
- Fixes mistakes that make it difficult for JavaScript engines to perform optimizations: **strict mode code can sometimes be made to run faster than identical code that's not strict mode.**
- Prohibits some syntax likely to be defined in future version of ECMAscript.

[^2]: [JavaScript 严格模式(use strict) | 菜鸟教程](https://www.runoob.com/js/js-strict.html)

## Reference

[前端面试每日 3+1-以前端面试题来驱动学习，提倡每日学习与思考，每天进步一点！](http://www.h-camel.com/index.html)

[Strict mode - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode)
