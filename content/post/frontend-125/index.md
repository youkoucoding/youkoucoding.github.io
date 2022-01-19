---
title: '毎日のフロントエンド  125'
date: 2022-01-19T16:12:31+09:00
description: frontend 每日一练
image: frontend-125-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第一百二十五日

## CSS

### **Question:** CSS 代码实现动画的暂停与播放

- 属性：`animation-play-state`

  - paused 暂停
  - running 播放

- `hover`取代点击

```css
.stop:hover ~ .animation {
  animation-play-state: paused;
}
```

- `checked`伪类
  - radio 标签的 checked 伪类，加上实现点击响应

```css
#stop:checked ~ .animation {
  animation-play-state: paused;
}
#play:checked ~ .animation {
  animation-play-state: running;
}
```

## JavaScript

### **Question:** `WebRTC API`

`WebRTC`[^1] (Web Real-Time Communications) 是一项实时通讯技术，它允许网络应用或者站点，在不借助中间媒介的情况下，建立浏览器之间点对点（Peer-to-Peer）的连接，实现视频流和（或）音频流或者其他任意数据的传输。WebRTC 包含的这些标准使用户在无需安装任何插件或者第三方的软件的情况下，创建点对点（Peer-to-Peer）的数据分享和电话会议成为可能。

[^1]: [WebRTC API - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/WebRTC_API)

## Clean Code JavaScript

### **Testing**

Testing is more important than shipping. If you have no tests or an inadequate amount, then every time you ship code you won't be sure that you didn't break anything. Deciding on what constitutes an adequate amount is up to your team, but having 100% coverage (all statements and branches) is how you achieve very high confidence and developer peace of mind. This means that in addition to having a great testing framework, you also need to use a **good coverage tool**.

There's no excuse to not write tests. There are plenty of good JS test frameworks, so find one that your team prefers. When you find one that works for your team, then **aim to always write tests for every new feature/module you introduce.** If your preferred method is Test Driven Development (TDD), that is great, but the main point is to just make sure you are reaching your coverage goals before launching any feature, or refactoring an existing one.

#### 1. Single concept per test

**Bad:**

```js
import assert from 'assert';

describe('MomentJS', () => {
  it('handles date boundaries', () => {
    let date;

    date = new MomentJS('1/1/2015');
    date.addDays(30);
    assert.equal('1/31/2015', date);

    date = new MomentJS('2/1/2016');
    date.addDays(28);
    assert.equal('02/29/2016', date);

    date = new MomentJS('2/1/2015');
    date.addDays(28);
    assert.equal('03/01/2015', date);
  });
});
```

**Good:**

```js
import assert from 'assert';

describe('MomentJS', () => {
  it('handles 30-day months', () => {
    const date = new MomentJS('1/1/2015');
    date.addDays(30);
    assert.equal('1/31/2015', date);
  });

  it('handles leap year', () => {
    const date = new MomentJS('2/1/2016');
    date.addDays(28);
    assert.equal('02/29/2016', date);
  });

  it('handles non-leap year', () => {
    const date = new MomentJS('2/1/2015');
    date.addDays(28);
    assert.equal('03/01/2015', date);
  });
});
```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[ryanmcdermott/clean-code-javascript: Clean Code concepts adapted for JavaScript](https://github.com/ryanmcdermott/clean-code-javascript#introduction)
