---
title: '毎日のフロントエンド　28'
date: 2021-10-14T17:36:17+09:00
description: frontend 每日一练
image: frontend-28-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第二十八日

## HTML

### **#Question:** 说说你对`<meta>`标签的理解

`meta`标签 **元数据(Metadata)**是`HTML`语言`<head>`区的一个辅助性标签，它位于 HTML 文档头部的`head`和`title`之间，它提供用户不可见的信息。

元数据可以被使用浏览器（如何显示内容或重新加载页面），搜索引擎（关键词），或其他 Web 服务调用

| 属性              | 值                                                           | 描述                                               |
| ----------------- | ------------------------------------------------------------ | -------------------------------------------------- |
| `charset (HTML5)` | `character_set`                                              | 定义文档的字符编码。                               |
| `content`         | `text`                                                       | 定义与 `http-equiv` 或 `name` 性相关的元信息。     |
| `http-equiv`      | `content-type、default-style、refresh`                       | 把 `content` 属性关联到 HTTP 头部。                |
| `name`            | `application-name、author、description、generator、keywords` | 把` content` 属性关联到一个名称                    |
| `scheme `         | `format/URI `                                                | HTML5 不支持。 定义用于翻译 content 属性值的格式。 |

## CSS

### **#Question:** `rgba()` 和 `opacity` 这两个的透明效果有什么区别呢

- `rgba` 只对颜色有影响。如果放在 `background` 上的话，只对背景颜色有影响。不会影响元素中的其他内容以及子元素内容

- `opacity` 的透明效果是作用整个元素以及其子元素上的

## JavaScript

### **#Question:** 解释下列段代码的意思

```js
// $$('*') 为获取所有 dom 元素，返回数组
[].forEach.call($$('*'), function (a) {
  // forEach 的回调函数，这里的 a 是数组中每个 dom 元素，不是 a 标签
  a.style.outline =
    // ～～是取整 1<<24 是位运算 结果为 16777216
    // 之后的 toString(16) 为进行 16 进制的转换 即颜色
    '1px solid #' + (~~(Math.random() * (1 << 24))).toString(16);
});
```

#### 作用

> 在你的 Chrome 浏览器的控制台中输入这段代码，你会发现不同 `HTML` 层都被使用不同的颜色添加了一个高亮的边框。简单来说，这段代码只是首先获取了所有的页面元素，然后使用一个不同的颜色为它们添加了一个 `1px` 的边框。

#### 解析

- `[].forEach.call()` => 调用引用数组的 forEach 方法
- `$$('*')` => `document.querySelectorAll('*')`
- `~~a` => `parseInt(a)`
- `1<<24` => 对二进数 1 小数点右移 24 位
- `(parseInt(Math.random()\*(1<<24)).toString(16))` => 获得了一个位于 0-16777216 之间的随机整数，也就是随机颜色，再使用 `toString(16)`将它转化为十六进制数。

```js
[].forEach.call(document.querySelectorAll('*'), function (a) {
  a.style.outline =
    '1px solid #' + parseInt(Math.random() * (1 << 24)).toString(16);
});
```

## Reference

[HTML meta 标签总结与属性使用介绍 - SegmentFault 思否](https://segmentfault.com/a/1190000004279791)

[<meta>：文档级元数据元素 - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/meta)

[从一行代码里面学点 JavaScript - L3ve 的绝对领域 - OSCHINA - 中文开源技术交流社区](https://my.oschina.net/l3ve/blog/330358)

[从输入 URL 到页面加载的过程？如何由一道题完善自己的前端知识体系！ | Dailc 的个人主页](https://dailc.github.io/2018/03/12/whenyouenteraurl.html)
