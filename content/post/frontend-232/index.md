---
title: '毎日のフロントエンド 232~240'
date: 2022-05-08T17:39:28+09:00
description: frontend 每日一练
categories:
  - HTML
  - CSS
  - Tips
---

## HTML

### table 由哪几部分组成

```html
<table></table>
<tbody></tbody>
<th></th>
<tr></tr>
<td></td>

<!-- 定义表格的标题 -->
<caption></caption>

<!-- 定义表格的页脚 -->
<tfoot></tfoot>

<!-- 定义表格的页眉 -->
<thead></thead>

<!-- 定义用于表格列的属性 -->
<col></col>

<!-- 定义表格列的组 -->
<colgroup></colgroup>
```

## CSS

### 模拟边框跟 border 效果一样

- `outline`, 伪元素, `box-shadow`

### class 和 id 选择器

- `id`具有唯一性，导致样式不能重用，**且权重大，容易导致权重问题**。不过用 id 来选择元素的效率比 class 高, 不推荐使用`id`选择器

### 兼容浏览器的前缀

- `-webkit-` 谷歌
- `-moz-` 火狐
- `-o-` opera
- `-ms-` ie

## Reference

- [haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

- [`outline` - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/outline)

- [`<table>` - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/table)
