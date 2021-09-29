---
title: '毎日のフロントエンド　15'
date: 2021-09-29T09:49:10+09:00
description: frontend 每日一练
image: frontend-15-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第十五日

## HTML

### **#Question:** `title` 与 `h1` 的区别、`b` 与 `strong` 的区别、`i` 与 `em` 的区别

- `title`: 网页的标题，tag 标签标题。
- `h1`： 网页内部的标题
- **Attention:** 如果 title 为空，但是页面存在 h1,b,strong 标签，搜索引擎会默认页面 title 为 h1 内的内容，所以 得出结论 h1 是在没有外界干扰下除 title 以外第二个能强调页面主旨的标记，在一个页面中应该使用且**只使用一次 h1 标记。**

- `b`： 加粗(bold)，是实体标签， 应当使用 CSS 而不是 `<b>`
- `strong`: 语义化的 b， 属于逻辑标签。
- **Attention:** 尽量使用 `strong`

- `i`： 斜体，是实体标签，应当使用 CSS 而不是 `<i>`
- `em`: 语义化的`i`，逻辑标签， `i`, `em` 同样表示强调，但是成都没有 `strong` 高
- **Attention:** 物理元素是告诉浏览器我应该以何种格式显示文字，逻辑元素告诉浏览器这些文字有怎么样的重要性。对于搜索引擎来说`em`和`strong`比 i 和 b 要重视。

## CSS

### **#Question:** `style`标签写在 body*前*和 body*后*的区别是什么？

- 写在 body 标签前利于浏览器逐步渲染
- 写在 body 标签后：由于浏览器以逐行方式对 html 文档进行解析；当解析到写在尾部的样式表（外联或写在 style 标签）会导致浏览器停止之前的渲染，等待加载且解析样式表完成之后重新渲染； 在 windows 的 IE 下可能会出现 FOUC 现象（即样式失效导致的页面闪烁问题）；

- 加载和执行的一些优化点

  - CSS 样式表置顶 （阻塞页面渲染）
  - 用 `<link>` 代替`@import` （`@import` 是 CSS，不会触发浏览器并发机制；在 CSS 加载完成后进行的引入。 但现代浏览器`@import` 和 `link` 在表现上已经没有上述区别了）
  - js 脚本置底（因为浏览器有并发限制，所以把 js 放到下边，减少占用的并发数，使得页面能够更快的渲染出来）合理使用 js 的异步加载能力

## JavaScript

### **#Question:** 写一个数组去重的方法（支持多维数组）

```js
function flatArr(arr, target) {
  arr.forEach((item) => {
    if (Array.isArray(item)) {
      flatArr(item, target);
    } else {
      target.push(item);
    }
  });
}

function midArr(arr) {
  let result = [];

  flatArr(arr, result);

  return result;
}

function uniqueArr(arr) {
  return [...new Set(midArr(arr))];
}

const result = uniqueArr([1, 2, 3, 4, [3, 4, [4, 6]]]);

console.log(result); // 1,2,3,4,6
```

---

```js
function uniqueArr(arr) {
  return [...new Set(arr.flat(Infinity))];
}
```

1. `flat()` method creates a new array with all sub-array elements concatenated into it recursively up to the specified depth.

2. `flat()` has two alternatives: `reduce` and `concat`

## Reference

[title タグと h1 タグは完全同一がいい? 違ってもいい? どう使い分ける? など 10+4 記事](https://webtan.impress.co.jp/e/2013/09/06/15972)

[文字を強調するタグ strong・b・em・i の違いと SEO 効果](https://nandemo-nobiru.com/2096/)

[Array.prototype.flat() - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flat)

[前端面试每日 3+1-以前端面试题来驱动学习，提倡每日学习与思考，每天进步一点！](http://www.h-camel.com/index.html)
