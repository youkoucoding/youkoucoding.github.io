---
title: '毎日のフロントエンド 333'
date: 2022-06-12T22:58:50+09:00
description: frontend 每日一练
categories:
  - CSS
  - JavaScript
---

## HTML

### img 标签的`onerror`事件

- img 标签中的 src 图片加载失败，变得美观

- `<img src="images/logo.png" onerror="javascript:this.src='images/logoError.png';">`

- 当图片 `images/logo.png` 不存在时，将触发 `onerror` 事件，而 onerror 中又为该 img 指定了 images/logoError.png 图片。也就是说图片 images/logo.png 存在则显示 logo.png，图片 images/logo.png 不存在将显示 logoError.png。

```html
<!-- 防止 endless loop -->
<script type="text/javascript">
  2     function imgerrorfun(){
  3         var img=event.srcElement;
  4         img.src="images/logoError.png";
  5         img.onerror=null; 控制不要一直跳动
  6     }
  7
</script>
8 9 <img src="images/logo.png" onerror="imgerrorfun();" />
```

## CSS

### `scroll-snap-align`

- 滚动一个列表时，控制列表中一个块始终完全在可视区内,如果滚动到一半可以回弹，保持整个块都在可视区

## JS

### js 实现 typeof

```js
function getType(v) {
  return v === undefined ? 'undefined' : v === null ? 'null' : v.constructor.name;
}
```

---

```js
const typeofFun = (value) => {
  const type = Object.prototype.toString
    .call(value)
    .replace(/\[object |]/g, '')
    .toLowerCase();
  const dic = ['undefined', 'number', 'boolean', 'symbol', 'string', 'function', 'bigint'];
  if (dic.includes(type)) return type;
  return 'object';
};
```

## Reference

- [fe-interview/history.md](https://github.com/haizlin/fe-interview/blob/master/category/history.md)
