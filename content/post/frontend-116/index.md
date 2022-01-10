---
title: '毎日のフロントエンド  116'
date: 2022-01-10T10:43:28+09:00
description: frontend 每日一练
image: frontend-116-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第一百一十六日

## HTML

### **Question:** 页面布局中的结构与表现分离，那么什么是结构？什么是表现

- 结构，是由 HTML 或者 XHTML 之类的标记语言负责创建，标签对网页内容的语义含义做出了描述
  - 例如： 标签表达了这“这是一个文本段落”，但只是表达了其语义，并没有包含关于内容如何显示的信息
- 表现，CSS 描述页面内容应该如何呈现
  - 例如： 可以定义这样一个 CSS 来声明：“文本段应该使用灰色的 Arial 字体和另外几张 scan-serif 字体来显示

## CSS

### **Question:** 自定义鼠标指针的图案

`cursor: url() ,auto`

- url 是自定义光标图案的绝对路径，auto 是默认光标，当我们自定义的光标不起作用时，就用默认光标代替

## JavaScript

### **Question:** 使用 js 来截图？怎样截可见区域和整个页面

[puppeteer/puppeteer: Headless Chrome Node.js API](https://github.com/puppeteer/puppeteer)

### **Question:** 前端加密的常见场景和方法

#### 场景-密码传输

- 前端加密、后端解密后计算密码字符串的 MD5/MD6 存入数据库；`Base64 / Unicode+1`
- 直接前端使用一种稳定算法加密成唯一值、后端直接将加密结果进行 MD5/MD6，全程密码明文不出现在程序中。直接使用 MD5/MD6 之类的方式取 Hash ，让后端存 Hash 的 Hash 。

#### 场景-数据包加密

`HTTPS`

#### 场景-展示成果加密

- 将文本内容进行展示层加密，利用字体的引用特点，把拿给爬虫的数据变成“乱码”

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[前端进阶](https://muyiy.cn/)

[Web 端反爬虫技术方案](https://juejin.cn/post/6844903654810468359)

[前端加密那点事](https://juejin.cn/post/6844903764973846542)

[brix/crypto-js: JavaScript library of crypto standards.](https://github.com/brix/crypto-js)
