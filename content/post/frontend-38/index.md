---
title: '毎日のフロントエンド　38'
date: 2021-10-24T10:07:50+09:00
description: frontend 每日一练
image: frontend-38-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第三十八日

## HTML

### **Question:** 说说你对 `cookie` 和 `session` 的理解

#### cookie

`HTTP Cookie`（也叫 `Web Cookie` 或浏览器 `Cookie`）是服务器发送到用户浏览器并保存在本地的一小块数据，它会在浏览器下次向同一服务器再发起请求时被携带并发送到服务器上。通常，它用于告知服务端两个请求是否来自同一浏览器，如保持用户的登录状态。Cookie 使基于无状态的 HTTP 协议记录稳定的状态信息成为了可能。

`cookie` 分发是通过扩展 `HTTP 协议`来实现的，服务器通过在 HTTP 的响应头`header`中加上一行特殊的指示以提示浏览器按照指示生成相应的 `cookie`。然而纯粹的客户端脚本如 `JavaScript` 等方式也可以生成 `cookie`。

cookie 的使用是由浏览器按照一定的原则在后台自动发送给服务器的。浏览器检查所有存储的 cookie，如果某个 cookie 所声明的作用范围大于等于将要请求的资源所在的位置，则把该 cookie 附在请求资源的 HTTP 请求头上发送给服务器。

Cookie 主要用于:

- 会话状态管理（如用户登录状态、购物车、游戏分数或其它需要记录的信息）
- 个性化设置（如用户自定义设置、主题等）
- 浏览器行为跟踪（如跟踪分析用户行为等）

创建 Cookie:

当服务器收到 HTTP 请求时，服务器可以在响应头里面添加一个 `Set-Cookie` 选项。浏览器收到响应后通常会保存下 Cookie，之后对该服务器每一次请求中都通过 `Cookie` 请求头部将 `Cookie` 信息发送给服务器。另外，`Cookie` 的过期时间、域、路径、有效期、适用站点都可以根据需要来指定。

#### Session

`cookie`机制弥补了 HTTP 协议无状态的不足。在 Session 出现之前，基本上所有的网站都采用 Cookie 来跟踪会话。

与 Cookie 不同的是，`session`是以**服务端保存状态**。

当客户端请求创建一个`session`的时候，服务器会先检查这个客户端的请求里是否已包含了一个`session标识 `- sessionId`:

- 如果已包含这个 sessionId，则说明以前已经为此客户端创建过 session，服务器就按照 sessionId 把这个 session 检索出来使用（如果检索不到，可能会新建一个）
- 如果客户端请求不包含 sessionId，则为此客户端创建一个 session 并且生成一个与此 session 相关联的 sessionId

#### What is Cookie?

A cookie us a small file with the maximum size of `4kb` that the web server stores on the `client`. Once a cookie has been set, all page requests that follow return the cookie name and value. **A cookie can only be read from the domain that it has been issued from.** Most web browsers have options for disabling cookies, third party cookies or both.

#### What is a Session?

A session is a global variable stored on the server. Each session is assigned a unique id which is used to retrieve stored values. Whenever a session is created, a cookie containing the unique session id is stored on the user's computer and returned with every request to the server. If the client browser does not support cookiesm the unique session id is displayed in the URL. Sessions have the capacity to store relatively large data compared to cookies.

The session values are automaticlly deleted when the browser is closed.

#### KEY Difference

- Cookies are client-side files that contain user information, whereas Sessions are server-side files that contain user information.
- Cookie is not dependent on session, but Session is dependent on Cookie.
- Cookie expires depending on the lifetime you set for it, while a Session ends when a user closes browser.
- The maximum cookie size is 4kb whereas in session, you can store as much data as you like.
- Cookie does not have a function named unsetcookie() while in Session you can use. Session_destroy(); which is used to destroy all registered data or to unset some.

#### Why and when to use Cookie?

Http is a stateless protocol; cookies allow us to track the state of the application using small files stored on the user's computer. The path were cookies are stored depends on the browser.

#### Why and when to use Session?

To store important information such as the user id more securely on the server where malicious users cannot temper with them. Sessions are used to pass values from one page to another.

It is also used when you want the alternative to cookies on browsers that do not support cookies, to store global variables in an efficient and more secure way compared to passing them in the URL, developing an application such as a shopping cart that has to temporary store information with a capacity large than 4kb.

## CSS

### **Question:** 实现单行文本居中和多行文本左对齐并超出显示"..."

```css
.one {
  text-align: center
}

.multi {
  overflow: hidden
  text-overflow: ellipsis
  display: -webkit-box
  -webkit-line-clamp: 3
  -webkit-box-orient: vertical
}
```

## JavaScript

### **Question:** 说说你对 eval 的理解

`eval()` 函数会将传入的字符串当做 JavaScript 代码进行执行。

The `eval()` function evaluates JavaScript code represented as a string.

> Executing JavaScript from a string is an enormous security risk. It is far too easy for a bad actor to run arbitrary code when you use eval().

- 不安全的, 容易出错
- 性能低
- 某种情况下跟 new Function(), setTimeout, setInterval 类似
- 不利于代码可维护性, 可拓展性
- 不是在无可奈何的情况下, 请不要使用

### **Question:** 一个给定字符串中，不重复出现的最长的子字符串 以及 这个子字符串的长度（第 4 题）

```js
function lengthOfStr(str) {
  let res = '';
  let result = [...str].reduce((acc, v, i) => {
    if (i === 0) {
      return v;
    } else {
      if (acc.indexOf(v) < 0) {
        return acc + v;
      } else {
        res = res.length < acc.length ? acc : res;
        return acc.slice(acc.indexOf(v) + 1, acc.length) + v;
      }
    }
  }, '');
  console.log(res.length > result.length ? res : result);
  return Math.max(res.length, result.length);
}
```

---

```js
let str = 'fdsfsdzffdfdddfsdsds';

let arr = [];
const s = str.split('');
let total = 0; // 长度
let maxStr = ''; // 最长不重复长度的字符串
for (let i = 0; i < s.length; i++) {
  const ele = s[i];
  const idx = arr.indexOf(ele);
  if (idx > -1) {
    arr = arr.slice(idx + 1);
  }
  arr.push(ele);
  if (arr.length > total) {
    maxStr = arr.join('');
    total = arr.length;
  }
}

console.log(total);
console.log(maxStr);
```

---

```js
/**
 * 题目：字符串出现的不重复最长长度
 * 整体思路：
 * 用一个滑动窗口装没有重复的字符，枚举字符记录最大值即可
 * 对于遇到重复字符如何收缩窗口大小？
 * 可以用 map 维护字符的索引，遇到相同的字符，把左边界移动过去即可
 * 挪动的过程中记录最大长度
 */
var lengthOfLongestSubstring = function (s) {
  let map = new Map();
  let i = -1;
  let res = 0;
  let n = s.length;
  for (let j = 0; j < n; j++) {
    if (map.has(s[j])) {
      i = Math.max(i, map.get(s[j]));
    }
    res = Math.max(res, j - i);
    map.set(s[j], j);
  }
  return res;
};
```

## Reference

[session 和 cookie - 掘金](https://juejin.cn/post/6844903957916024839)

[详解 cookie、session、webStorage - 掘金](https://juejin.cn/post/6844903953029677064)

[Cookies and Sessions - cs142](https://web.stanford.edu/~ouster/cgi-bin/cs142-fall10/lecture.php?topic=cookie)

[HTTP cookies - HTTP | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Cookies)

[Difference between Cookie and Session](https://www.guru99.com/difference-between-cookie-session.html)

[lgwebdream/FE-Interview: Html、Css、JavaScript、Vue、React、Node、TypeScript、Webpack、算法、网络与安全、浏览器](https://github.com/lgwebdream/FE-Interview)
