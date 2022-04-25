---
title: '毎日のフロントエンド 215'
date: 2022-04-25T21:24:44+09:00
description: frontend 每日一练
categories:
  - CSS
  - Tips
---

## CSS

### 图片宽度自适应

- `object-fit: contain;`

## Tips

### TypeScript

- The _"noUncheckedIndexedAccess"_ is the most awesome config option you've never heard of. It makes accessing objects a lot safer, and also powers up TypeScript's inference on Objects.

```ts
export const myObj: Record<string, string[]> = {};

myObj.foo.push('bar'); // get an error
```

- Solution

```json
// tsconfig.json

{
  "noUncheckedIndexedAccess": true
}
```

```ts
export const myObj: Record<string, string[]> = {};

if (!myObj.foo) {
  myObj.foo = [];
}

myObj.foo.push('bar');
```

## Reference

- [haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

- [object-fit - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/object-fit)

- [ Full Screen API - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Fullscreen_API)

- [ Recordkeys - Utility Types](https://www.typescriptlang.org/docs/handbook/utility-types.html#recordkeys-type)

- [noUncheckedIndexedAccess - tsc CLI Options](https://www.typescriptlang.org/docs/handbook/compiler-options.html#handbook-content)
