---
title: '毎日のフロントエンド 318~320'
date: 2022-06-03T22:12:23+09:00
description: frontend 每日一练
categories:
  - CSS
  - JavaScript
---

## HTML

## CSS

### `* + *{ ... }`

```css
li + li {
  margin-top: 1rem;
}
```

- 相邻兄弟元素(向下查找，自然第一个不是任何一个元素的相邻兄弟元素)
- 可以给除了第一个 li 以外的 li 指定样式，比如在两个 li 之间加上 margin
- 等价于，如下

```css
li:not(:first-of-type) {
  margin-top: 1rem;
}
```

---

- `+`： 相邻兄弟选择器（Adjacent sibling selector）可选择紧接在另一元素后的元素，且二者有相同父元素
- `*`：通配符,代表任意元素

- 避免首尾元素产生不必要的影响

## Tips

### `UUID`

- UUID 含义是通用唯一识别码 (Universally Unique Identifier)，
  - 是一个软件建构的标准，也是被开源软件基金会 (Open Software Foundation, OSF)的组织应用在分布式计算环境 (Distributed Computing Environment, DCE) 领域的一部分。
- `Math.random().toString(32).slice(2)`

## Reference

- [fe-interview/history.md](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

- [JS 生成 UUID 的四种方法 ](https://www.cnblogs.com/zhou195/p/7498537.html)
