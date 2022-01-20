---
title: '毎日のフロントエンド  126'
date: 2022-01-20T12:12:13+09:00
description: frontend 每日一练
image: frontend-126-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第一百二十六日

## HTML

### **Question:** Canvas 和 SVG

- canvas

  - 依赖分辨率
  - 不支持事件处理器
  - 基于 js 绘制的
  - 弱的文本渲染能力
  - 能够以 .png 或 .jpg 格式保存结果图像
  - 最适合图像密集型的游戏，其中的许多对象会被频繁重绘

- svg
  - 不依赖分辨率
  - 支持事件处理器，可为其中个别节点绑定事件
  - 基于 xml 进行定义的，因此 svg 有节点的概念，可对其中部分节点绑定事件
  - 最适合带有大型渲染区域的应用程序（比如谷歌地图）
  - 复杂度高会减慢渲染速度（任何过度使用 DOM 的应用都不快）
  - 不适合游戏应用

## Clean Code JavaScript

### Concurrency

#### 1. Use promises not callbacks

Callbacks aren't clean, and they cause excessive amounts of nesting. With ES2015/ES6, Promises are a built-in global type. Use them!

**Bad:**

```js
import { get } from 'request';
import { writeFile } from 'fs';

get(
  'https://en.wikipedia.org/wiki/Robert_Cecil_Martin',
  (requestErr, response, body) => {
    if (requestErr) {
      console.error(requestErr);
    } else {
      writeFile('article.html', body, (writeErr) => {
        if (writeErr) {
          console.error(writeErr);
        } else {
          console.log('File written');
        }
      });
    }
  }
);
```

**Good:**

```js
import { get } from 'request-promise';
import { writeFile } from 'fs-extra';

get('https://en.wikipedia.org/wiki/Robert_Cecil_Martin')
  .then((body) => {
    return writeFile('article.html', body);
  })
  .then(() => {
    console.log('File written');
  })
  .catch((err) => {
    console.error(err);
  });
```

#### 2. Async/Await are even cleaner than Promises

Promises are a very clean alternative to callbacks, but `ES2017/ES8` brings `async` and `await` which offer an even cleaner solution. All you need is a function that is prefixed in an async keyword, and then you can write your logic imperatively without a then chain of functions. Use this if you can take advantage of ES2017/ES8 features today!

**Bad:**

```js
import { get } from 'request-promise';
import { writeFile } from 'fs-extra';

get('https://en.wikipedia.org/wiki/Robert_Cecil_Martin')
  .then((body) => {
    return writeFile('article.html', body);
  })
  .then(() => {
    console.log('File written');
  })
  .catch((err) => {
    console.error(err);
  });
```

**Good:**

```js
import { get } from 'request-promise';
import { writeFile } from 'fs-extra';

async function getCleanCodeArticle() {
  try {
    const body = await get('https://en.wikipedia.org/wiki/Robert_Cecil_Martin');
    await writeFile('article.html', body);
    console.log('File written');
  } catch (err) {
    console.error(err);
  }
}

getCleanCodeArticle();
```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[ryanmcdermott/clean-code-javascript: Clean Code concepts adapted for JavaScript](https://github.com/ryanmcdermott/clean-code-javascript#introduction)
