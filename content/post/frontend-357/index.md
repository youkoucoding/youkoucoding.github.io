---
title: '毎日のフロントエンド 357'
date: 2022-07-04T21:33:50+09:00
description: frontend 每日一练
categories:
  - CSS
  - Tips
---

## CSS

### `input` 元素 `size` 属性和 `width` 的区别

- `size`
  - 控件的初始大小。以像素为单位。但当 type 属性为 text 或 password 时， 它表示输入的字符的长度。从 HTML5 开始， 此属性仅适用于当 type 属性为 text, search, tel, url, email，或 password；否则会被忽略。 此外，它的值必须大于 0。 如果未指定大小，则使用默认值 20。 HTML5 概述 "用户代理应该确保至少大部分字符是可见的", 但是不同的字符的用不同的字体表示可能会导致宽度不同。在某些浏览器中，一串带有 x 的字符即使定义了到 x 的大小也将显示不完整。
- `width` 与 `<img>`的`width`相同
  - 如果 type 属性的值是 image，这个属性定义了按钮图片的宽度
  - 图像的宽度，在单位是 CSS 像素
  - 如果 size 和 style='width：xx' 同时使用的时候，最终由 style='width：xx' 来决定（行内样式优先）

### `background-blend-mode`

- 定义该元素的背景图片，以及背景色如何混合

## Tips

### `RESTful` `GraphQL` `Webhooks` `gRPC`

#### `REST`

- 无状态的，以资源为核心，针对如何操作资源定义了一系列 URL 约定
- 操作类型通过 `GET` `POST` `PUT` `DELETE` 等 HTTP Methods 表示

---

- REST 基于原生 HTTP 接口，因此改造成本很小，而且其无状态的特性，降低了前后端耦合程度，利于快速迭代
- 随着未来发展，REST 可能更适合提供微服务 API

使用举例：

```bash
curl -v -X GET https://api.sandbox.paypal.com/v1/activities/activities?start_time=2012-01-01T00:00:01.000Z&amp;end_time=2014-10-01T23:59:59.999Z&amp;page_size=10 \
-H "Content-Type: application/json" \
-H "Authorization: Bearer Access-Token"
```

#### `gRPC`

- gRPC 是对 RPC 的一个新尝试，最大特点是使用 protobufs 语言格式化数据
- RPC 主要用来做服务器之间的方法调用，影响其性能最重要因素就是 序列化/反序列化 效率。RPC 的目的是打造一个高效率、低消耗的服务调用方式，因此比较适合 IOT 等对资源、带宽、性能敏感的场景。而 gRPC 利用 protobufs 进一步提高了序列化速度，降低了数据包大小。

---

使用举例：

- gRPC 主要用于服务之间传输，以 Nodejs 为例：

1. 定义接口。由于 gRPC 使用 protobufs，所以接口定义文件就是 `helloworld.proto`:

```js
// The greeting service definition.
service Greeter {
  // Sends a greeting
  rpc SayHello (HelloRequest) returns (HelloReply) {}
  // Sends another greeting
  rpc SayHelloAgain (HelloRequest) returns (HelloReply) {}
}

// The request message containing the user's name.
message HelloRequest {
  string name = 1;
}

// The response message containing the greetings
message HelloReply {
  string message = 1;
}
```

- �� 定义了服务 Greeter，拥有两个方法：SayHello 与 SayHelloAgain，通过 message 关键字定义了入参与出参的结构。
- 利用 `protobufs`，传输数据时仅传送很少的内容，作为代价，双方都要知道接口定义规则才能序列化/反序列化

2. 定义服务器：

```js
function sayHello(call, callback) {
  callback(null, { message: 'Hello ' + call.request.name });
}

function sayHelloAgain(call, callback) {
  callback(null, { message: 'Hello again, ' + call.request.name });
}

function main() {
  var server = new grpc.Server();
  server.addProtoService(hello_proto.Greeter.service, {
    sayHello: sayHello,
    sayHelloAgain: sayHelloAgain,
  });
  server.bind('0.0.0.0:50051', grpc.ServerCredentials.createInsecure());
  server.start();
}
```

- 在 50051 端口支持了 gRPC 服务，并注册了服务 Greeter，并对 sayHello sayHelloAgain 方法做了一些业务处理，并返回给调用方一些数据

---

3. 定义客户端：

```js
function main() {
  var client = new hello_proto.Greeter('localhost:50051', grpc.credentials.createInsecure());
  client.sayHello({ name: 'you' }, function (err, response) {
    console.log('Greeting:', response.message);
  });
  client.sayHelloAgain({ name: 'you' }, function (err, response) {
    console.log('Greeting:', response.message);
  });
}
```

- 客户端和服务端同时需要拿到 proto 结构，客户端数据发送也要依赖 proto 包提供的方法，框架会内置做掉序列化/反序列化的工作

#### `GraphQL`

- GraphQL 不是 REST 的替代品，而是另一种交互形式：**前端决定后端的返回结果**

- GraphQL 带来的最大好处是精简请求响应内容，不会出现冗余字段，前端可以决定后端返回什么数据。
- 前端的决定权取决于后端支持什么数据，因此 GraphQL 更像是精简了返回值的 REST
- 后端接口也可以一次性定义完所有功能，而不需要逐个开发

---

- 相比 REST 和 gRPC，GraphQL 是由前端决定返回结果的反模式

使用举例：

```graphql
query {
  organization(login: "github") {
    members(first: 100) {
      edges {
        node {
          name
          avatarUrl
        }
      }
    }
  }
}
```

- 返回的结果和约定的格式结构一致，且不会有多余的字段：

```js
{
  "data": {
    "organization": {
      "members": {
        "edges": [
          {
            "node": {
              "name": "Chris Wanstrath",
              "avatarUrl": "https://avatars0.githubusercontent.com/u/2?v=4"
            }
          },
          {
            "node": {
              "name": "Justin Palmer",
              "avatarUrl": "https://avatars3.githubusercontent.com/u/25?v=4"
            }
          }
        ]
      }
    }
  }
}
```

#### `Webhooks`

- Webhooks 可以说是彻头彻尾的反模式了，因为其定义就是，前端不主动发送请求，**完全由后端推送**
  - 最适合解决轮询问题。或者说轮询就是一种妥协的行为，当后端不支持 Webhooks 模式时
- 对于第三方平台验权、登陆等 没有前端界面做中转的场景，或者强安全要求的支付场景等，适合用 Webhooks 做数据主动推送。

---

- REST：无状态的数据传输结构，适用于通用、快速迭代和标准化语义的场景
- gRPC：轻量的传输方式，特殊适合对性能高要求或者环境苛刻的场景，比如 IOT
- GraphQL: 请求者可以自定义返回格式，某些程度上可以减少前后端联调成本
- Webhooks: 推送服务，主要用于服务器主动更新客户端资源的场景

## Reference

- [fe-interview/history.md](https://github.com/haizlin/fe-interview/blob/master/category/history.md)
- [`<input>` | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/Input)
- [background-blend-mode - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-blend-mode)
- [When to Use What: REST, GraphQL, Webhooks, & gRPC | Nordic APIs](https://nordicapis.com/when-to-use-what-rest-graphql-webhooks-grpc/)
- [GitHub GraphQL API - GitHub Docs](https://docs.github.com/cn/graphql)
- [apollographql/apollo-client:  A fully-featured, production ready caching GraphQL client for every UI framework and GraphQL server.](https://github.com/apollographql/apollo-client)
