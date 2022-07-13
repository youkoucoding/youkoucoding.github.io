---
title: '毎日のフロントエンド 365~367'
date: 2022-07-12T22:29:38+09:00
description: frontend 每日一练
categories:
  - HTML
  - CSS
  - JS
  - Tips
---

## HTML

### `innerHTML`与`outerHTML`

- `innerHTML`获取的是对应 id 里面的 html 字符串
- `outerHTML`获取的是对应 id 包括子节点的 html 字符串

## JS

### 深度优先遍历和广度优先遍历

```js
const data = [
  {
    name: 'a',
    children: [
      { name: 'b', children: [{ name: 'e' }] },
      { name: 'c', children: [{ name: 'f' }] },
      { name: 'd', children: [{ name: 'g' }] },
    ],
  },
  {
    name: 'a2',
    children: [
      { name: 'b2', children: [{ name: 'e2' }] },
      { name: 'c2', children: [{ name: 'f2' }] },
      { name: 'd2', children: [{ name: 'g2' }] },
    ],
  },
];

//深度优先
function byDeepth(data) {
  let result = [];
  const map = (item) => {
    result.push(item.name);
    item.children && item.children.forEach((item) => map(item));
  };
  data.forEach((item) => {
    map(item);
  });
  return result.join(',');
}

//广度优先
function byBreadth(data) {
  let result = [];
  let queque = data;
  while (queque.length > 0) {
    result.push(queque[0].name);
    if (queque[0].children) {
      queque = queque.concat(queque[0].children);
    }
    queque.shift();
  }
  return result.join(',');
}
console.time('深度优先');
console.log('深度优先', byDeepth(data));
console.timeEnd('深度优先');

console.time('广度优先');
console.log('广度优先', byBreadth(data));
console.timeEnd('广度优先');
```

---

```js
深度优先 a,b,e,c,f,d,g,a2,b2,e2,c2,f2,d2,g2
VM166:48 深度优先: 0.141357421875 ms
VM166:51 广度优先 a,a2,b,c,d,b2,c2,d2,e,f,g,e2,f2,g2
VM166:52 广度优先: 0.0849609375 ms
```

## Tips

### Chrome devtools tip 1: Copy CSS styles as JavaScript compatible properties

1. Inspect an element via DevTools.
2. Right click on the styles within the Styles Pane.
3. Select Copy all declarations as JS (or copy a single declaration).

```js
const anchor = document.querySelector('a');

Object.assign(anchor.style, {
  color: 'white',
  letterSpacing: '2px',
});
```

## Reference

- [fe-interview/history.md](https://github.com/haizlin/fe-interview/blob/master/category/history.md)
- [Dev Tips - Developer Tips by Umar Hansa](https://umaar.com/dev-tips/)
- [Copy CSS as JavaScript - Chrome DevTools - Dev Tips](https://umaar.com/dev-tips/249-copy-css-as-js/)
