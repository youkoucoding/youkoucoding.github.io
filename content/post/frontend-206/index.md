---
title: '毎日のフロントエンド 206'
date: 2022-04-14T23:39:27+09:00
description: frontend 每日一练
categories:
  - HTML
  - CSS
  - Tips
---

## HTML

### 移动端点击 300ms 的延迟出现的原因

- 早期 IOS 为了区分用户是双击缩放还是点击链接行为，于是就有了 300ms 延迟，其他浏览器效仿
  - 引入`fastclick`
  - 在`meta`禁用浏览器缩放
  - `touch`事件模拟

## CSS

### 元素设置 background-color,它的颜色会填充哪些区域

- `background`
  - 填充区域默认为`content`、`padding`和`border`区域
  - 该行为由`background-clip`属性决定，默认为`border-box`
    - `background-clip` 设置元素的背景（背景图片或颜色）是否延伸到边框、内边距盒子、内容盒子下面
- 填充区域如下

|  background-clip  | margin | border | padding | content | text |
| :---------------: | :----: | :----: | :-----: | :-----: | :--: |
| border-box (默认) |   ✘    |   ✔    |    ✔    |    ✔    |  -   |
|    padding-box    |   ✘    |   ✘    |    ✔    |    ✔    |  -   |
|    content-box    |   ✘    |   ✘    |    ✘    |    ✔    |  -   |
|   text (webkit)   |   ✘    |   ✘    |    ✘    |    ✘    |  ✔   |

## JavaScript

### js 获取 DOM 元素的方法

- `document.getElementById` - 元素的 ID 在大部分情况下要求是独一无二的，这个方法自然而然地成为了一个高效查找特定元素的方法
  - 返回一个匹配到 ID 的 DOM Element 对象, 没有找到，则返回 `null`
  - 仅在 document 下调用
- `document.getElementsByName` - 根据给定的 name (en-US) 返回一个在 (X)HTML document 的节点列表集合
  - 返回值是一个实时更新的 `NodeList` 集合。当文档中有同一个 name 属性的元素被添加或移除时，这个集合会自动更新
- `document.getElementsByClassName`
  - 返回一个包含了所有指定**类名**的**子元素的类数组对象**
  - `getElementsByClassName` **可以在任何元素上调用**，不仅仅是 document。 _调用这个方法的元素将作为本次查找的根元素_
- `document.getElementsByTagName`
  - 返回一个包括所有给定标签名称的元素的 HTML 集合 `HTMLCollection`
  - 整个文件结构都会被搜索，包括根节点
  - 返回的 HTML 集合是动态的, 意味着它可以自动更新自己来保持和 DOM 树的同步而不用再次调用 `document.getElementsByTagName()`
- `document.querySelector`
  - 文档对象模型 Document 引用的 querySelector()方法返回文档中与指定选择器或选择器组匹配的**第一个 Element 对象**
  - 如果找不到匹配项，则返回 null。
- `document.querySelectorAll`
  - 返回与指定的选择器组匹配的文档中的元素列表 (使用深度优先的先序遍历文档的节点)
  - 返回的对象是 NodeList
- `document.body`
  - 返回当前文档中的`<body>`元素或者`<frameset>`元素.
- `document.documentElement`
  - 返回文档对象（document）的根元素的**只读属性**（如 HTML 文档的 `<html>` 元素）

## Reference

- [haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

- [fastclick 的介绍和使用 - 简书](https://www.jianshu.com/p/67bae6dfca90)

- [ftlabs/fastclick: Polyfill to remove click delays on browsers with touch UIs](https://github.com/ftlabs/fastclick)

- [background - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background)

- [background-clip - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-clip)

- [Element - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Element)

- [NodeList - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/NodeList)

- [HTMLCollection - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLCollection)

- [document.querySelector() - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/querySelector)
