---
title: '毎日のフロントエンド 276'
date: 2022-05-20T23:50:25+09:00
description: frontend 每日一练
categories:
  - HTML
  - Tips
---

## HTML

### html 的标签元素分为哪几大类

#### HTML 文档标签

- `<!DOCTYPE>`：定义文档类型
- `<html>`： 定义 HTML 文档
- `<head>`：定义文档的头部
  - `<meta>`：定义元素可提供的有关页面的元信息，比如针对搜索引擎和更新频度的描述和关键字
  - `<base>`：定义页面上的所有链接规定默认地址或默认目标
  - `<title>`：定义文档标题
  - `<link>`：定义文档与外部资源的关系
  - `<style>`：定义 HTML 文档样式信息
- `<body>`：定义文档的主体。（脚本在非必需情况时在<body>的最后）
  - `<script>`：定义客户端脚本，比如 javascript。
  - `<noscript>`：定义脚本未被执行时的替代内容。（如文本）

#### 按闭合特征分类

##### 空标签

- `<br> <hr> <img> <input> <link> <meta>`

#### 是否换行

##### 块级元素

- 块级元素的特点：
  - 每个块级元素独占一行，每个块级元素都会从新的一行开始，从上到下排布
  - 块级元素可以直接控制宽度、高度以及盒子模型的相关 css 属性
  - 在不设置宽度的情况下，块级元素的宽度是他父级元素内容的宽度
  - 在不设置高度的情况下，块级元素的高度是他本身内容的高度

##### 内联元素

- 内联元素的特点：
  - 内联元素会和其他元素从左到右显示在一行
  - 内联元素不能直接控制宽度、高度以及盒子模型的相关 css 属性，但是可以设置内外边距的水平方向的值。
  - 对于内联元素的 margin 和 padding，只有 margin-left/margin-right 和 padding-left/padding-right 是有效的，但是竖直方向的 margin 和 padding 无效果
  - 内联元素的宽高是由内容本身的大小决定的（文字、图片等）
  - 内联元素只能容纳文本或者其他内联元素（不要在内联元素中嵌套块级元素）

### 前端调试技巧

#### Must Read

- [Dev Tips - Developer Tips by Umar Hansa](https://umaar.com/dev-tips/)

- [Debugging Tips and Tricks | CSS-Tricks - CSS-Tricks](https://css-tricks.com/debugging-tips-tricks/?utm_source=javascriptweekly&utm_medium=email)

## Reference

- [fe-interview/history.md](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

- [HTML 标签分类总结 - 简书](https://www.jianshu.com/p/5a78a19f18bf)

- [weekly/11.精读《前端调试技巧》.md at master · ascoders/weekly](https://github.com/ascoders/weekly/blob/master/%E5%89%8D%E6%B2%BF%E6%8A%80%E6%9C%AF/11.%E7%B2%BE%E8%AF%BB%E3%80%8A%E5%89%8D%E7%AB%AF%E8%B0%83%E8%AF%95%E6%8A%80%E5%B7%A7%E3%80%8B.md)
