---
title: React17 + ReactHook + TS4 仿Jira项目---学习笔记（二）
date: 2021-09-09T20:17:16+09:00
description: 知识点： RESTful API
image: cover-api.jpg
categories:
  - React
  - TypeScript
  - RESTful API
---

# 关键知识点四： Restful API

## What is REST

REST is acronym for REpresentational State Transfer. It is architectural style for distributed hyermedia systems and was first presented by Roy Fielding in 2000 in his famous dissertation.

## Principles of REST

1. **Client-server**

   - By separating the user interface concerns from the data storage concerns, we improve the portability of the user interface across multiple platforms and improve scalability by simplifying the server components.

2. **Stateless**

   - Each request from client to server must contain all of the information necessary to understand the request, and cannot take advatage of any stored context on the server. Session state is therefore kept entirely on the client.

3. **Cacheable**

   - Cache constraints require that the data within a response to a request be implicitly or explicitly labeled as cacheable or non-cacheable. If a response is cacheable, then a client cache is given the right to reuse that response data for later, equivalent requests.

4. **Uniform interface**（统一接口）

   - By applying the software engineering principle of generality to the component interface, the overall system architecture is simplified and the visibility of interactions is improved. In order to obtain a uniform inerface, multiple architectural constraints are needed to guide the behavior of components. REST is defined by four interface constraints: _identification of resources_; _manipulation of resources through representations_; _self-descriptive messages_; and, _hypermedia as the engine of application state_.

   - identification of resources :使用`URI`作为资源标识符；客户端和服务器之间，传递这种资源的某种表现层（representation）; 资源本身与返回给 client 的 representation 分开(理解为：URI 只代表资源的实体或资源的位置， 不代表其形式，比如一段文本，可以是 json，HTML，etc)。例如，服务器不直接发送其数据库内容，而是发送一些表示某些数据库记录的 HTML，XML 或 JSON。
     _具体表现形式，应该在 HTTP 请求头信息中用 Accept 和 Content-Type 字段指定，这两个字段才是对"表现层"的描述_

   - manipulation of the resources through representations: 当客户端持有资源的表示（包括附加的任何元数据）时，它有足够的信息来修改或删除服务器上的资源

   - Self-descriptive Messages （自描述信息）： 每条消息都包含足够的信息来描述如何处理该消息。

   - hypermedia as the engine of application state: client 通过 body/header/query_params/uri_name 来提供状态，服务器收到这些内容，通过 HTTP 响应状态码和响应头向服务端提供状态，成为超链接。且，在必要时，链接包含在返回的正文（或标题）中，以提供用于检索对象本身或相关对象的 URI。

5. **Layered system**

   - The layered system style allows an architecture to be composed of hierarchical layers by constraining component behavior such that each component cannot "see" beyond the immedite layer with which they are interacting.

   - 客户端通常无法判断它是直接连接到终端服务器，还是中间服务器。 中间服务器可以通过启用负载平衡和提供共享缓存来提高系统可伸缩性。 Layers 也可以实施安全策略。

6. **Code on demand(optional)**(唯一一个可选约束，其他约束必须实现)

   - REST allows client functionality to be extended by downloading and executing code in the form of applets or scripts(JS etc). This simplifies clients by reducing the number of features required to be pre-implemented.

## Resource

> The key abstraction of information in REST is a resource.

Any information that can be named can be a resource. REST uses a **resource indentifier** to identify the particular resource involved in an interaction between comonents.

REST 的名称"表现层状态转化"中，省略了主语。"表现层"其实指的是"资源"（Resources）的"表现层"。

所谓"资源"，就是网络上的一个实体，或者说是网络上的一个具体信息。它可以是一段文本、一张图片、一首歌曲、一种服务，总之就是一个具体的实在。你可以用一个 URI（统一资源定位符）指向它，每种资源对应一个特定的 URI。要获取这个资源，访问它的 URI 就可以，因此 URI 就成了每一个资源的地址或独一无二的识别符。

## Representation

"资源"是一种信息实体，它可以有多种外在表现形式。我们把"资源"具体呈现出来的形式，叫做它的"表现层"（Representation）。

## State Transfer

互联网通信协议 HTTP 协议，是一个无状态协议。这意味着，所有的状态都保存在服务器端。因此，如果客户端想要操作服务器，必须通过某种手段，让服务器端发生"状态转化"（State Transfer）。而这种转化是建立在表现层之上的，所以就是"表现层状态转化"。具体如下：

### Resource Method

> Another important thing associated with REST is resource methods to be used to perform the desired transition. A large number of people wrongly relate resource methods to HTTP GET/PUT/POST/DELETE methods.

- **GET** 用来获取资源
- **POST** 用来新建资源（也可以用于更新资源）
- PUT 用来更新资源
- DELETE 用来删除资源

> In simplest words, in the REST architectural style, data and functionality are considered resources and are accessed using Uniform Resource Identifiers (URIs). The resources are acted upon by using a set of simple, well-defined operations. The clients and servers exchange representations of resources by using a standardized interface and protocol – typically HTTP.

> Resources are decoupled from their representation so that their content can be accessed in a variety of formats, such as HTML, XML, plain text, PDF, JPEG, JSON, and others. Metadata about the resource is available and used, for example, to control caching, detect transmission errors, negotiate the appropriate representation format, and perform authentication or access control. And most importantly, every interaction with a resource is stateless.
