---
title: '毎日のフロントエンド  113'
date: 2022-01-07T15:17:10+09:00
description: frontend 每日一练
image: frontend-113-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第一百一十三日

## HTML

### **Question:** 在 HTML5 中如何组合标题？用哪个元素

`<hgroup>`

- HTML `<hgroup>` element 代表文档章节所属的多级别的目录，它将多个<h1>至<h6>的子元素组装到一起。
- 对区段（section）的标题进行组合

## CSS

### **Question:** element、class 和 id 选择器三者的区别是什么？分别在什么时候用

标签选择器的格式为 `element { ... }`，class 选择器的格式为 `.class-name { ... }`，id 选择器的格式为 `#identifier { ... }`

### **Question:** 3 ways to elegantly center a div (horizontally and vertically)

```css
.centered {
  display: grid;
  place-items: center;
}

.also-centered {
  display: flex;
  justify-content: center;
  align-items: center;
}

.centered-absolute {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}
```

## JavaScript

### **Question:** 用 js 写一个事件侦听器的方法

```js
/**
 * 一个简单的发布订阅者模式
 */

interface IHandler {
  fn: Function;
  type: string;
  name: string;
}

export default class EventUtils {
  /**
   * 键值对 对应事件名称以及数组的值
   */
  static handler = {};

  /**
   * on 方法 添加监听事件
   */
  static on(name: string, handler: Function): EventUtils {
    const i: IHandler = {
      fn: handler,
      type: 'on',
      name: name,
    };
    if (Object.keys(EventUtils.handler).includes(name)) {
      EventUtils.handler[name].push(i);
      return EventUtils;
    }
    EventUtils.handler[name] = [].concat(i);
    return EventUtils;
  }

  /**
   * off 方法 移除监听事件
   */
  static off(name: string, handler: Function): EventUtils {
    const event: any[] = EventUtils.handler[name];
    if (event) {
      for (let i = event.length - 1; i >= 0; i--) {
        if (event[i].fn === handler) {
          event.splice(i, 1);
        }
      }
    }
    return EventUtils;
  }

  /**
   * emit 方法 触发监听的事件
   */
  static emit(name: string, ...args: any): EventUtils {
    const event = EventUtils.handler[name];
    let newEvent = [];
    event &&
      event.length &&
      event.forEach((item: IHandler, index: number) => {
        item.fn.call(this, ...args);

        // 如果有只监听一次的事件
        if (item.type !== 'once') {
          newEvent.push(event.slice(index, index + 1));
        }
      });

    const hasOnce =
      event &&
      event.length &&
      event.some((item: IHandler) => {
        return item.type === 'once';
      });

    if (hasOnce) {
      EventUtils.handler[name] = newEvent;
    }

    // 这里做一个执行完成之后的 once代码 off 的操作
    return EventUtils;
  }

  /**
   * once 方法 添加事件 只会被执行一次
   */
  static once(name: string, handler: Function): void {
    EventUtils.on(name, handler);
    EventUtils.handler[name][0]['type'] = 'once';
  }
}
```

### **Question:** shim 和 polyfill 有什么区别？它们分别有什么用

> `polyfill`是`shim`的一种.目的都是在没有相关 API 的环境上顺利执行代码

- shim:是 js 库, 抹平不同平台的差异性，提供一致对外的接口调用

  - 用库定义的 api 去执行一些操作, 如 jquery,$.ajax,底层用 ActiveXObject 或 XMLHttpRequest 实现,用户不直接用标准 API,而是用库定义的 api,具体环境判断和实现交给库

- polyfill: 用 js 模拟标准 api，抹平环境差异，用户仍是使用标准 api(在旧版浏览器上提高可用性)

  - 比如发现`window.requestAnimationFrame`不存在,polyfill 会用 setTimeout 覆写`window.requestAnimationFrame`

- polyfill 的好处在于，可以动态监测浏览器版本,低版本加载,而在高本版浏览器可以不加载这段 js，用户代码不修改直接可用

- shim 则是无论浏览器版本都要加载这段 js，因为用的 api 是库定义的

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[前端进阶](https://muyiy.cn/)

[`<hgroup>` - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/hgroup)

[zloirock/core-js: Standard Library - polyfill](https://github.com/zloirock/core-js)
