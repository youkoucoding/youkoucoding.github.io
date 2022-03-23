---
title: '毎日のフロントエンド 185'
date: 2022-03-23T14:28:43+09:00
description: frontend 每日一练
categories:
  - Tips
---

## Tips

### `onload`和`domcontentloaded`哪个先执行

- 当初始的 HTML 文档被完全加载和解析完成之后，`DOMContentLoaded` 事件被触发，而**无需等待样式表、图像和子框架的完全加载**。
- `onload` 需要所有资源全都加载完成才执行

### 组件就是基于视图的模块

> 组件的核心任务就是将数据渲染到视图并监听用户在视图上的操作

#### 虚拟 DOM

- **优化性能** 。DOM 操作是比较耗时的，对于大量、频繁的 DOM 操作，如果先在 JavaScript 中模拟进行，然后再通过计算比对，找到真正需要更新的节点，这样就有可能减少不必要的 DOM 操作，从而提升渲染性能。但并不是所有的 DOM 操作都能通过虚拟 DOM 提升性能，比如单次删除某个节点，直接操作 DOM 肯定比虚拟 DOM 计算比对之后再删除要快。总体而言， **虚拟 DOM 提升了 DOM 操作的性能下限，降低了 DOM 操作的性能上限**。 所以会看到一些对渲染性能要求比较高的场景，比如在线文档、表格编辑，还是会使用原生 DOM 操作。

- **跨平台** 。由于虚拟 DOM 以 JavaScript 对象为基础，所以可根据不同的运行环境进行代码转换（比如浏览器、服务端、原生应用等），这使得它具有了跨平台的能力。

#### 数据模型(React)

- React 组件的数据模型 state，其值就是 **对象类型**。
  - React 并没有直接采用深拷贝的方式来实现，因为深拷贝操作性能开销太大。
- React 组件是通过将 state 设置为不可变对象的方式来实现的，不可变对象指的就是当一个变量被创建后，它的值不可以被修改。
  - 意味着当组件状态发生变化时，不修改 state 属性，而是重新创建新的 state 状态对象。
- React 中的不可变对象通过 Structural Sharing（结构共享）的操作，大大减少了性能开销。
  - 操作的原理就是，如果对象中的一个属性发生变化，那么只深拷贝当前属性，然后将对象属性指向这个深拷贝的属性，其他节点仍然进行共享。

#### 渲染

- React 组件中的视图更新，并不是像 Vue 中那样自动响应的，而是需要手动调用 setState() 函数来触发。
  - React 为了提升组件更新时的性能，不仅将状态更新包装成任务放入了**异步**队列，而且还使用了类似协程的方式来调度这些队列中的更新任务。
  - 任务的执行顺序会根据每个任务的优先级来进行调整，并且任务的执行过程中可能会被中断，但状态会被保存，直到合适的时候会再次读取状态并继续执行任务。

---

- Vue 采取的是响应式的视图更新方式，基于 `Object.defineProperty()` 函数，监听数据对象属性的变化，然后再更新到视图。
- Vue 在组件初始化的时候会将 `data()` 函数返回的数据对象传入 `observe()` 函数，在这个函数中会将数据对象作为参数来创建一个 `Observer` 实例，在这个实例的构造函数中将会通过 `Object.defineProperty` 为数据对象的每个属性设置监听。

### 前端路由实现基础

> 默认情况下，当地址栏的 URL 发生变化时，浏览器会向服务端发起新的请求。所以实现前端路由的重要基础就是在修改 URL 时，不引起浏览器向后端请求数据。根据浏览器提供的 API，有两种实现方案。

#### 基于 hash 实现

- 当 URL 变化时浏览器会发送请求，但有一种特例，那就是 hash 值的变化不会触发浏览器发起请求
- hash 值是指 URL“`#`”号后面的内容，通过 `location.hash` 属性可以读写 hash 值，这个值可以让浏览器将页面滚动到 ID 与 hash 值相等的 DOM 元素位置，不会传给服务端
- 通过监听 `window` 对象的 `hashchange` 事件就可以感知到它的变化
- 这种实现方式占用了 hash 值，导致默认的页面滚动行为失效，对于有滚动定位需求的情况需要自行手动获取元素的位置并调用 BOM 相关 API 进行滚动

#### 基于 history 实现

> `HTML 5` 提出了一种更规范的前端路由实现方式，通过 `history` 对象

- `history` 提供了两个函数来修改 URL

  - `history.pushState()`
  - `history.replaceState()`
  - 两个 API 可以在不进行刷新的情况下，来操作浏览器的历史 记录 。唯一不同的是，前者是新增一个历史记录，后者是直接替换当前的历史记录。

- 监听 URL 变化则可以通过监听 window 对象上的 `popstate` 事件来实现

  - `history.pushState()` 或 `history.replaceState()` **不会触发** `popstate` 事件，这时需要手动触发页面渲染

    > 虽然能通过这种方式实现前端路由功能，但并不能拦截浏览器默认的请求行为，当用户修改地址栏网址时还是会向服务端发起请求，所以还需要服务端进行设置，将所有 URL 请求转向前端页面，交给前端进行解析

    - vue-router 官网的 Nginx 配置例子：表示对于匹配的路径，按照指定顺序依次检查对应路径文件是否存在，对应路径目录是否存在，如果没有找到任何文件 或目录 ，就返回 index.html。而 index.html 就会引入对应的 JavaScript 代码在浏览器端进行路由解析。

```nginx
location / {
  try_files $uri $uri/ /index.html;
}
```

#### 路由解析

> 阻止浏览器在 URL 变化时向后端发送请求之后，就需要对路由进行解析了。 vue-router 和 react-router 都同时依赖了一个第三方库 Path-to-RegExp 进行路由解析，下面通过分析 path-to-regexp 1.8 版本的源码来理解路由是如何被解析的。

- 路由解析又分为两个操作：**路由匹配**和**路由生成**。

---

- 基于 hash 方式兼容性较好，但是占用了浏览器默认的定位行为，同时会加长 URL 字符串；- 基于 history 方式可以直接修改 URL 路径，较为美观。

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[webNavigation.onDOMContentLoaded - Mozilla 产品与私有技术 | MDN](https://developer.mozilla.org/zh-CN/docs/Mozilla/Add-ons/WebExtensions/API/webNavigation/onDOMContentLoaded)

[GlobalEventHandlers.onload - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/GlobalEventHandlers/onload)

[pillarjs/path-to-regexp: Turn a path string such as `/user/:name` into a regular expression](https://github.com/pillarjs/path-to-regexp)
