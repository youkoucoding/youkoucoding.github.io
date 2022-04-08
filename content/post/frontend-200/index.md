---
title: '毎日のフロントエンド 200'
date: 2022-04-08T22:18:38+09:00
description: frontend 每日一练
categories:
  - HTML
  - CSS
  - Tips
---

## HTML

### `<p>`和`<br>`的区别

- `<p>`块级元素，`<br>` 内联元素；
- `<p>`能被 css 修改，`<br>` 不能；
- `<p>`非单标签元素，`<br>` 是单标签元素；
- `<p>`换行靠的是块级元素特性，`<br>` 换行靠的可能是类似 `\n` 的渲染规则

## Tips

### JavaScript

- Accessing URL data using window.location in JavaScript

```js
//https://www.typescriptlang.org/docs/handbook/2/mapped-types.html#mapping-modifiers

window.location.hash;
// '#mapping-modifiers'

window.location.href;
// 'https://www.typescriptlang.org/docs/handbook/2/mapped-types.html#mapping-modifiers'

window.location.origin;
// 'https://www.typescriptlang.org'

window.location.pathname;
// '/docs/handbook/2/mapped-types.html'

window.location.search;
// ''
```

### TypeScript

- Transform a union to another union, using the `in` operator as a kind of for-loop

```typescript
type Entity =
  | {
      type: 'user';
    }
  | {
      type: 'post';
    }
  | {
      type: 'comment';
    };

// to implement below:

type EntityWithId =
  | {
      type: 'user';
      userId: sting;
    }
  | {
      type: 'post';
      postId: string;
    }
  | {
      type: 'comment';
      commentId: string;
    };
```

- solution:

```typescript
type EntityWithId = {
  [EntityType in Entity['type']]: {
    type: EntityType;
  } & Record<`${EntityType}Id`, string>;
}[Entity['type']];

const result: EntityWithId = {
  type: 'comment',
  commentId: '12345',
};
```

- [Playground Link](https://www.typescriptlang.org/play?#code/C4TwDgpgBAogdsAlqKBeAsAKClAPlAbyxxKlEgC4oByAVwGcIAnagbmJIF8P8jtSy4CFWpgA9vWBsOObv14yS5YTQDGYgLYaICaf1ntMWZbATIQAdWQALAJIATNIQ4BteElAAVIVERxTHiAu1MrUALphVHykylTu5t6QhrJQAGRQAEoQ6kz2ADwABgAkBPFeQpwOBQA0UJJMfgDmAHyGnG5moMGhEYZY6nCSUEwQ9LQANsBxnZY2Dk7RgpRqmtq61RzqWjrADiIAjABMAMwALACs1BuYnKxAA)

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)
