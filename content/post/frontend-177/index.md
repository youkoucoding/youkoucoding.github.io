---
title: '毎日のフロントエンド  177'
date: 2022-03-15T11:28:16+09:00
description: frontend 每日一练
categories:
  - HTML
  - CSS
  - Tips
---

## HTML

### 网站首页有大量的图片，加载很慢，怎么优化

1. 小图用 iconfont（svg）代替。
2. 不能替代的用 base64。
3. 去除 gif 图，用 video 代替。
4. 工具压缩图片的大小。
5. 优先使用 webp 格式
6. 骨骼屏+ 懒加载。

## CSS

### `preload`、`preconnect`、`prefetch`

- `Preload`: 在 `<link>` 标签中使用 preload 时，提前请求资源。主要用于获取当前路由中使用的高优先级资源。
  - 元素的 rel 属性的属性值 preload 能够让你在你的 HTML 页面中元素内部书写一些声明式的资源获取请求，可以指明哪些资源是在页面加载完成后即刻需要的。
  - `rel="preolad"`声明这是一个 preload
  - `href` 指明资源的位置
  - `as` 指明资源类型（这是为了让浏览器精确设置优先级，设置正确的 CSP、Accept 头部）
  - `crossorigin` 指明使用的跨域设置
  - 添加 preload 声明之后，浏览器初次加载的资源变多了，但 preload 并不会阻塞 onload 事件的触发
- `Preconnect`: 解决 DNS 和 TCP 握手问题, 使浏览器能够预先建立一个连接，等真正需要加载资源的时候就能够直接请求了
- `Prefetch`: 提前获取资源将其置于缓存中，使用资源时从缓存中获取而不是发出另一个请求。

## Tips

### `debounce` (TypeScript)

- 函数 `debounce` 添加类型，包括参数类型和返回值类型。参数类型使用泛型变量，在调用函数 `debounce` 的时候手动指定，**泛型变量有 3 个：函数 T 、函数 T 的返回值 U 和 函数 T 的参数 V**
- 变量 timeout ，当定时器存在时它的值为 number，定时器不存在时值为 null

```ts
const debounce = <T extends Function, U, V extends any[]>(func: T, wait: number = 0) => {
  let timeout: number | null = null;
  let args: V;
  function debounced(...arg: V): Promise<U> {
    args = arg;
    if (timeout) {
      clearTimeout(timeout);
      timeout = null;
    }
    // 以promise形式返回函数执行结果
    return new Promise({resolve, reject}=>{
      timeout = setTimeout(async ()=>{
        try {
          const result:U = await func.apply(this, args)
          resolve(result)
        } catch(e){
          reject(e)
        }
      }, wait)
    })
  }

  // 可以取消debounce
  function cancel(){
    clearTimeout(timeout)
    timeout = null
  }
  // 允许立即执行
  function flush():U{
    cancel()
    return func.apply(this, args)
  }

  debounced.cancel = cancel;

  debounced.flush = flush;

  return debounced;
};
```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[TypeScript: Documentation - More on Functions](https://www.typescriptlang.org/docs/handbook/2/functions.html#function)
