---
title: '毎日のフロントエンド 212'
date: 2022-04-22T21:57:53+09:00
description: frontend 每日一练
categories:
  - HTML
  - CSS
  - Tips
---

## HTML

### 如何自动转移到新的页面

- `<head>`内引入

- `<meta http-equiv="refresh" content="0; url=http://example.com/">`
  - content 内第一个参数是延迟，单位秒，0 为立即跳转
  - 参数 url 是跳转地址

## Tips

### DRY Use `typeof import('./')` to grab the type of any module, even third-party ones.

```ts
// src/demo/constants.ts
export const ADD_TODO = 'ADD_TODO';
export const REMOVE_TODO = 'REMOVE_TODO';
export const EDIT_TODO = 'EDIT_TODO';
```

```ts
export type ActionModule = typeof import('./contants');

export type Action = ActionModul[keyof ActionModule];
// type Action = 'ADD_TODO' | 'REMOVE_TODO' | 'EDIT_TODO'
```

## Reference

- [haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

- [HTTP 的重定向 - HTTP | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Redirections#html_%E9%87%8D%E5%AE%9A%E5%90%91%E6%9C%BA%E5%88%B6)
