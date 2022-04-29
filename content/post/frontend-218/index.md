---
title: '毎日のフロントエンド 218'
date: 2022-04-28T23:30:17+09:00
description: frontend 每日一练
categories:
  - CSS
  - Tips
---

## CSS

### CSS 下兼容性的元素水平/垂直翻转实现

```css
/*水平翻转*/
.flipx {
  -moz-transform: scaleX(-1);
  -webkit-transform: scaleX(-1);
  -o-transform: scaleX(-1);
  transform: scaleX(-1);
  /*IE*/
  filter: FlipH;
}

/*垂直翻转*/
.flipy {
  -moz-transform: scaleY(-1);
  -webkit-transform: scaleY(-1);
  -o-transform: scaleY(-1);
  transform: scaleY(-1);
  /*IE*/
  filter: FlipV;
}
```

- 对于基于 webkit 核心的浏览器，如 Chrome 以及 Safari，实现元素的垂直翻转或是水平翻转也可以使用如下样式:

```css
/*水平翻转*/
.flipx {
  transform: rotateY(180deg);
}

/*垂直翻转*/
.flipy {
  transform: rotateX(180deg);
}
```

1. 水平翻转或垂直翻转不同于旋转 180 度。前者以**轴**为镜像，后者以**点**为镜像;
2. 如果是对称元素，旋转 180 度和翻转的显示效果基本上就是一致的，但非对称元素就会看到明显差异;
3. 目前仅 webkit 核心浏览器支持的 `rotateY(180deg)`，大写的 `Y`, 这里的 Y 表示元素*以纵轴为镜像翻转，也就是水平翻转*

## Tips

### TypeScript

- **Assertion functions inside classes**

- Here, we assert that the user is logged in and get proper inference on the user's logged in user id

```ts
import { createPost } from './createPost';

export class SDK {
  constructor(public loggedInUserId?: string) {}

  createPost(title: string) {
    this.assertUserIsLoggedIn();

    createPost(this.loggedInUserId, title);
  }

  assertUserIsLoggedIn(): asserts this is this & {
    loggedInUserId: string;
  } {
    if (!this.loggedInUserId) {
      throw new Error('user is not logged in');
    }
  }
}
```

## Reference

- [haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

- [scale() - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform-function/scale)

- [rotate() - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform-function/rotate)

- [CSS 垂直翻转/水平翻转提高 web 页面资源重用性](https://www.zhangxinxu.com/wordpress/2011/05/css%E5%AE%9E%E7%8E%B0%E5%90%84%E4%B8%AA%E6%B5%8F%E8%A7%88%E5%99%A8%E5%85%BC%E5%AE%B9%E7%9A%84%E5%9E%82%E7%9B%B4%E7%BF%BB%E8%BD%AC%E6%B0%B4%E5%B9%B3%E7%BF%BB%E8%BD%AC%E6%95%88%E6%9E%9C/)
