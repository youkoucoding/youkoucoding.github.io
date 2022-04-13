---
title: '毎日のフロントエンド 205'
date: 2022-04-13T22:06:42+09:00
description: frontend 每日一练
categories:
  - Tips
---

## HTML

### `picture`标签

- `<picture> `元素通过包含零或多个 `<source>` 元素和一个 `<img>` 元素来为不同的显示/设备场景提供图像版本。浏览器会选择最匹配的子 `<source>` 元素，如果没有匹配的，就选择 `<img>` 元素的 src 属性中的 URL。然后，所选图像呈现在<img>元素占据的空间中。

- `<picture>` 的常见使用场景：
  - 艺术指导 (Art direction) —— 针对不同 media 条件裁剪或修改图像
  - 遇到所有浏览器都不支持的特定格式时，提供不同的图像格式

## JavaScript

### `getElementById`和`querySelector`方法的区别

- `Document`的方法 `getElementById()`返回一个匹配特定 ID 的元素. 由于元素的 ID 在大部分情况下要求是**独一无二**的，这个方法自然而然地成为了一个*高效查找特定元素的方法*。
- Document 引用的 querySelector()方法返回文档中与指定选择器或选择器组匹配的第一个 Element 对象。 如果找不到匹配项，则返回 null。
  - `selectors`:包含一个或多个要匹配的选择器的 DOM 字符串 DOMString。 该字符串必须是有效的 CSS 选择器字符串；如果不是，则引发 SYNTAX_ERR 异常。

## Tips

### TypeScript

- The Looseness of Object.keys can be a real pain point when it comes to use typescript.
- Use Generics and `keyof` keyword to create a tighter version.

```ts
const myObject = {
  a: 1,
  b: 2,
  c: 3,
};

Object.keys(myObject).forEach((key) => {
  console.log(myObject[key]);
});
```

- Solution

```ts
const myObject = {
  a: 1,
  b: 2,
  c: 3,
};

const objectKeys = <Obj>(obj: Obj): (keyof Obj)[] => {
  return Object.keys(obj) as (keyof Obj)[];
};

objectKeys(myObject).forEach((key) => {
  console.log(myObject[key]);
});
```

- [Playground Link](https://www.typescriptlang.org/play?jsx=0&ts=4.6.2#code/MYewdgzgLgBAtgTwPICMBWBTYsC8MDeAsAFAwwCGAXDAIwA0JZK1ATA6TMNQMzsC+JEqEiwQ6LFADSGBBBh4APKjQA+ABRi01ZQEpqagNYyQAMxi6A2gF15KgoxgAnDFACujsOfHYAdEdka6DoUcobGZpZWANwkfDHEJJoS0gGIyhI6PiYgjgCi5MAAFmphCME4dkQcwhAgADYYPnUgAOZqad5QFv5WOvF8fUA)

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[`<picture>`：picture 元素 - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/picture)

[`<img>`：图像嵌入元素 - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/img#attr-srcset)

[document.getElementById - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/getElementById)

[document.querySelector() - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/querySelector)

[TypeScript: as ](https://www.typescriptlang.org/docs/handbook/jsx.html#the-as-operator)
