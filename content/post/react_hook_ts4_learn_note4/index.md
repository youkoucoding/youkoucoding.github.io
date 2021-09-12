---
title: React17 + ReactHook + TS4 仿Jira项目---学习笔记（四）
date: 2021-09-11T15:14:04+09:00
description: 知识点： RESTful API Best Practise
image: cover-for-bestpractise.jpg
categories:
  - React
  - TypeScript
  - RESTful API
---

# 关键知识点六： Restful API 设计的几个最佳实践

## steps toward the glory of REST

[Richardson Maturity Model](https://martinfowler.com/articles/richardsonMaturityModel.html#level0)

### Level 0

The Starting point for the model is using HTTP as a transport system for remote interactions, but without using any of the mechanisms of the web.If you use SOAP or XML-RPC it's basically the same mechanism, the only difference is that you wrap the XML messages in some kind of envelope.

### Level 1 - Resources

At level One, rather than making all the requests to s singular service endpoint, we start talking to **individual resources**.

### Level 2 - Http Verbs (Method)

Level 2 moves away from being used as tunneling mechanisms allowing you to tunnel your interactions through HTTP, using the `http verbs` as closely as possible to how they are used in Http itself.

HTTP defines `GET` as a safe operation, that is it doesn't make any significant changes to the state of anything.--- This allows us to invoke GETs safely any number of times in any order and get the same results each time.([幂等 - 术语表 | MDN](https://developer.mozilla.org/zh-CN/docs/Glossary/Idempotent))

THE KEY elements that are supported by the existence of the web are the strong separation between safe (eg: GET) and non-safe operations, together with using status codes to help communicate the kinds of errors we run into.

### Level 3 - Hypermedia Controls

THE _Highest_ and final level introduces something that you often hear referred to under the おかしい acronym of HATEOAS (Hypertext As The Engine Of Application State).

Each response has a link element which contains a URI to tell us how to do next, and the URI of the resource we need to manipulate to do it.

One obvious benefit of hypermedia controls is that it allows the server to change its URI scheme without breaking clients. As long as clients look up the "add-test" link URI then the server can juggle all URIs other than the initial entrypoint.

A further benefit is that it helps client developers explore the protocal. The links give client developers a hint as to what may be possible next. AND simillarly it also allows the server team tp advertise new capabilities by putting new links in the responses.

So as a frontend developer, if we can keeping an eye out for unknown links, these links can be a trgger for further exploration.

## Best Practise 最佳实践

### 一， 一类资源两个 URL - Use Two URLs per Resource

```shell
# 资源集合：
/epics
# 资源元素：
/epics/5
```

### 二， 使用一致的复数名词 - Use Consistently Plural Nouns

只应该使用统一的**复数名词**来表达资源

```shell
GET /stories
GET /stories/3
```

### 三， 资源 URI 使用名词而不是动词 - Use Nouns instead of Verbs for Resources

```shell
# wrong
/getAllEpics
/getAllFinishedEpics
/createEpic
/updateEpic
```

[RESTful API Design.HTTP METHOD](https://phauer.com/2015/restful-api-design-best-practices/#http-methods)

```shell
# right
GET /epics
GET /epics?state=finished
POST /epics
PUT /epics/5
```

### 四， 将实际数据包装在 data 字段中

```json
// GET /epics在数据字段中返回epic资源列表
{
  "data": [
    { "id": 1, "name": "epic1" },
    { "id": 2, "name": "epic2" }
  ]
}
```

```json
// GET /epic/1在数据字段中返回id为1的epic对象
{
  "data": {
    "id": 1,
    "name": "epic1"
  }
}
```

> PUT，POST 和 PATCH 请求的有效负荷(payload)还应包含实际对象的数据字段。

> [JSON:API — A specification for building APIs in JSON](https://jsonapi.org/)

### 五， 对可选及复杂参数使用查询字符串

```shell
# 保持URL简单, 使用基本URL，将复杂或可选参数移动到查询字符串。
GET /employees?state=internal&title=senior
GET /employees?id=1,2

# 还可以使用JSON API方式过滤
GET /employees?filter[state]=internal&filter[title]=senior
GET /employees?filter[id]=1,2
```

### 六， 使用 HTTP 状态码 status codes

[HTTP response status codes - HTTP | MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)

状态码的使用要尽量精确。如果资源可用，但禁止用户访问，则返回 403。如果资源曾经存在但现已被删除或停用，请使用 410。

### 七， 提供有用的错误消息

```json
// request
GET /epics?state=unknow

// response
{
  "errors": [
    {
      "status": 400,
      "detail": "Invalid state. Valid values are 'biz' or 'tech'",
      "code": 352,
      "links": {
        "about": "http://www.jira.com/rest/errorcode/352"
      }
    }
  ]
}
```

### 八，HATEOAS - Provide Links for Navigating through your API

```json
// request
GET /epic
// 好的做法是在响应中提供客户可以跟进的链接
// response
{
  "data": [
    {
      "id": 1,
      "name": "epic1",
      "links": [
        {
          "story": "http://www.domain.com/epics/21/stories"
        }
      ]
    }
  ]
}
```

优点：

1. 如果 API 被更改，客户端依旧会获取有效的 URL（只要保证在 URL 更改时更新链接）
2. API 变得更具自描述性，客户端不必经常查找文档

### 九， 恰当地设计关系

在 API 中设计关系基本上有三种常用选项：链接，侧载和嵌入。

There are basically three common options to design relationships within an API: **Links**, **Sideloading** and **Embedding**.

Basically, you should design the relationships depending on the **client's access schema** and the **tolerable request amount** and **payload size**.

#### Links:

```json
{
  "data": [
    {
      "id": 1,
      "name": "Larry",
      "relationships": {
        "manager": "http://www.domain.com/employees/1/manager",
        "teamMembers": [
          "http://www.domain.com/employees/12",
          "http://www.domain.com/employees/13"
        ]
        //or "teamMembers": "http://www.domain.com/employees/1/teamMembers"
      }
    }
  ]
}
```

- _Small payload size._ : It's **good**, if the client doesn't need the manager and the teamManager every time.

- _Many Request_. : It's **bad**, if nearly every client needs this data. MANY additional requests may be required; in the worse case for every employee.

- The client has to stitch the data together in order to get the big picture.

#### Sideloading:

We can refer to the relationship with a foreign key and put the referred entitiese also in the payload but under the dedicated field `included.` This approach also called "Compound Documents".

```json
{
  "data": [
    {
      "id": 1,
      "name": "Larry",
      "relationships": {
        "manager": 5,
        "teamMembers": [12, 13]
      }
    }
  ],
  "included": {
    "manager": {
      "id": 5,
      "name": "Kevin"
    },
    "teamMembers": [
      { "id": 12, "name": "Albert" },
      { "id": 13, "name": "Tom" }
    ]
  }
}
```

The client may also control the sideloaded entities by a query parameter like `GET /employee?include=manager,teamMembers`.

- One singel request.
- Tailored payload size. No duplication (e.g. you only deliver a manager once even if he is referenced by many employees).
- The client still has to stitch the data together(拼接数据) in order to resolve the relationships. which can be very cumbersome.

#### Embedding:

```json
{
  "data": [
    {
      "id": 1,
      "name": "Larry",
      "manager": {
        "id": 5,
        "name": "Kev"
      },
      "teamMembers": [
        { "id": 12, "name": "Albert" },
        { "id": 13, "name": "Tom" }
      ]
    }
  ]
}
```

- Most conveninet for the client. It's can directly follow the relationships to get the actual data.

- Relationships may be loaded in vain if the client does not need it.

- Increased payload size and duplications. Referenced entities may be embedded multiple times.(not so good)

### 十， 小驼峰法命名属性 - Use CamelCase for Attribute Names

`{"yearOfBirth": 1970}`

Don not use underscores or capitalize (year_of_birth or YearOfBirth). Often the RESTful api will be consumed by a client written in JS. Typically the client wil convert the JSON response to a JS object( `var person = JSON.parse(response)` ) and call its attributes. Therefore, it's a good idea to stick to the JS convention which makes the js code more readable and intuitive.

```js
// Don't
person.year_of_birth; // violates JavaScript convention
person.YearOfBirth; // suggests constructor method

// Do
person.yearOfBirth;
```

### 十一，用动词表示操作 - Use Verbs for Operations

有时对 API 调用的响应不涉及资源（如计算，转义或变换）。

Sometimes a response to an API call does not involve resources (like calculate, translate, or convert)

```js
//Reading
GET /translate?from=de_DE&to=en_US&text=Hallo
GET /calculate?para2=23&para2=432

//Trigger an operation that changes the server-side state
POST /restartServer

//no body
POST /banUserFromChannel
{ "user": "123", "channel": "serious-chat-channel" }
```

In this case, no resource are involved. Instead, the server executes an operation and returns the result to the client.

Hence, we shoud use verbs instead of nouns in our URL to distinguish clearly the operations (RPC-style API) from the RESTful endpoints(resources for modelling the domain.)

Creating those RPC-style API instead of REST is appropriate for operations. Usualy, it's simpler and more intuitive than trying to be REST for operations (e.g. PATCH /server with `{"restart": true}`). _AS the rule of thumb_, REST is nice for interacting with domain models and RPC is suitable for operations.

> [Understanding RPC Vs REST For HTTP APIs — Smashing Magazine](https://www.smashingmagazine.com/2016/09/understanding-rest-and-rpc-for-http-apis/)

### 十二， 分页 Provide Pagination

It is almost never a good idea to return the whole data of your db at once.

Consequently, you should provide a pagination mechanism. There are TWO popular approaches below:

- Offset-based Pagination

- Keyset-based Pagination (Continuation Token) aka CURSOR----- recommended.

#### Offset-based

```fish
# use the parameters offset and limit, which are well-known from database

/employees?offset=30&limit=15 # returns the employees 30 to 45

# when the client omits the parameter, the server team should use default(like offset=0 and limit=100)
```

> Never return all resources.

You can provide links for getting the next or previous page. Just construct URLs with the appropriate offset and limit.

```shell
GET /employees?offset=20&limit=10
```

```json
{
  "pagination": {
    "offset": 20,
    "limit": 10,
    "total": 3465
  },
  "data": [
    //...
  ],
  "links": {
    "next": "http://www.domain.com/employees?offset=30&limit=10",
    "prev": "http://www.domain.com/employees?offset=10&limit=10"
  }
}
```

#### Keyset-based Pagination aka continuation TOKEN, CURSOR (recommend)

The presented offset-based pagination is easy to implement but comes with drawbacks. _They are slow (SQL’s OFFSET clause becomes very slow for large numbers) and unsafe (it’s easy to miss elements when changes are happening during pagination)._

> - SQL’s OFFSET clause becomes very slow for large numbers
> - it’s easy to miss elements when changes are happening during pagination

```shell
# That’s why it’s better to use an indexed column.
# Let’s assume that our employees have an indexed column data_created and the collection resource /employees?pageSize=100 returns the oldest 100 employees sorted by this column.
# Client only has to take the dateCreated timestamp of the last employee and uses the query parameter createdSince to continue at this point.

GET /employees?pageSize=100
# The client receives the oldest 100 employees sorted by `data_created`
# The last employee of the page has the `dataCreated` field  with 1504224000000 (= Sep 1, 2017 12:00:00 AM)

GET /employees?pageSize=100&createdSince=1504224000000
# The client receives the next 100 employees since 1504224000000.
# The last employee of the page was created on 1506816000000. And so on.
```

This solves already many of the disadvantages of offset-based pagination, but it’s still not perfect and not very convenient for the client.

- It’s better to create a so-called continuation token by adding additional information (like the id) to the date in order to improve the reliability and efficiency.
- Moreover, you should provide a dedicated field in the payload for that token so the client doesn’t have to figure it out by looking at the elements. You can even go further and provide a next link.

> [Web API Pagination with the 'Timestamp_ID' Continuation Token](https://phauer.com/2018/web-api-pagination-timestamp-id-continuation-token/)

```json
// request
GET /employees?pageSize=100

// response
{
  "pagination": {
    "continuationToken": "1504224000000_10"
  },
  "data": [
    // ...
    // last element:
    { "id": 10, "dateCreated": 1504224000000 }
  ],
  "links": {
    "next": "http://www.domain.com/employees?pageSize=100&continue=1504224000000_10"
  }
}
```

The `next` link makes the API RESTful as the client can page through the collection simply by following these links(HATEOAS). No need to construct URLs manually. Moreover, we can simply change the URL structure without breaking clients( called evolvability)

### 十三， Check out JSON:API

> [JSON:API — A specification for building APIs in JSON](https://jsonapi.org/)

Just for Inspiration. Feel free to make up your own mind about JSON:API.

### 十四， 确保 API 的可演进 Ensure Evolvability of the APIs

#### Avoid Breaking Changes

Ideally, APIs should be stable. Basically, breaking changes should not happen.(like change the whole payload format or the URL scheme). SO how can we still evolve our API without breaking the clients:

- 保持向后兼容 Make backward-compatible changes.Adding field is no problem, as long as the clients are tolerant.

- 复制和弃用。Duplication and Deprecation. In order to cahnge an existing field, you can add the new one next to the old field and deprecated the old one in the documentation. After a while, you can remove the old field.

- 超媒体和 HATEOAS。Utilize Hypermedia and HATEOAS. As long as the API client uses the links in the response to navigate through the API (and doesn’t craft the URLs manually), you can safely change the URLs without breaking the clients.

- 使用新名称创建新资源。Create new resources with new names. If new business requirements lead to a completely new domain model and workflows, you can create new resources. That’s often quite intuitive as the domain model has a new name anyway (derived from the business name). Example: A rental service now also rents bikes and segways. So the old concept car with the resource /cars doesn’t cut it anymore. A new domain model vehicle with a new resource /vehicles is introduced. It’s provided along with the old /cars resource.

#### Keep Bussiness Logic on the Server-Side

DO not let our service become _a dump data access layer_ which provides CRUD functionality by directly exposing your databaes model. **THIS creates HIGH COUPLING.**

- The bussiness logic is shifted to the client and is often replicated between the client and the server (just think about validation.). We have to keep both in sync.

- The client will be coupling to the server's database model. This is not good.

- The business workflows are getting distributed between the client and the server. IN TURN, that makes it likely that new business requirements require a change in both the client and the server and to break API. So the API/system is not that evolvable.

因此，我们应该构建高层次/基于工作流的 API 而不是低级 API。 So we should build high-level/workflow-based APIs instead of low-level APIs. EXAMPLE:

**Don’t provide a simple CRUD service for the order entities in the database.**

**Don’t require the clients to know that to cancel an order, the client has to PUT an order to the generic /order/1 resource with a certain cancelation payload (reflecting the database model) in it. This leads to high coupling (business logic and domain knowledge on the client-side; exposed database model).**

**Instead, provide a dedicated resource /order/1/cancelation and add a link to it in the payload of the order resource. The client can navigate to the cancelation URL and send a tailored cancelation payload. The business logic for mapping this payload to the database model is done in the server.**

**Moreover, the server can easily change the URL without breaking the client, because the client simply follows links. Besides, the decision logic, if an order can be canceled or not is now in the server: If a cancelation a possible the server adds the link to the cancelation resource in the order payload. So the client only has to check if the cancelation links are present (for example to know if he should draw the cancelation button). So we moved domain knowledge away from the client back to the server. Changes to the cancelation conditions can be easily applied by only touching the server, which in turn make the system evolvable. No API change is required.**

### 十五，版本化 Consider API Versioning

Nevertheless, you might end up in situations where the above approaches don’t work and you really have to provide different versions of your API.

Nevertheless, here are the two most popular approaches for versioning:

- Versioning via URLs: `/v1/`

- Versioning via the `Accept` HTTP Header:
  ```shell
   Accept:
   application/vnd.myapi.v1+json
  ```

#### Versioning via URLs

Just put the version number of your API in the URL of every resource. `/v1/books`.

- Pros:

  - Extremely simple for API developers.
  - Extremely simple for API clients.
  - URLs can be copied and pasted.

- Cons:
  - Not RESTful
  - Breaking URLs. clients have to maintain and update the URLs.

> Strictly speaking, this approach is not RESTful because URLs should never change.The question is, how much effort would it take the clients to update the URLs? If the answer is “only a little” then URL versioning might be fine.

#### Versioning via `Accept` HTTP Header (Content Negotiation)

- Pros:

  - URLs keep the same
  - Considered as RESTful
  - HATEOAS-friendly

- Cons:
  - Slightly more difficult to use. Clients have to pay attention to the headers.
  - URLs can’t be copied and pasted anymore.

## Reference

[Richardson Maturity Model](https://martinfowler.com/articles/richardsonMaturityModel.html#level0)

[Restful API 设计最佳实践 - Tech For Fun](http://kaelzhang81.github.io/2019/05/24/Restful-API%E8%AE%BE%E8%AE%A1%E6%9C%80%E4%BD%B3%E5%AE%9E%E8%B7%B5/)

[RESTful API Design. Best Practices in a Nutshell.](https://phauer.com/2015/restful-api-design-best-practices/)

[REST beyond the obvious – API design for ever evolving systems by Oliver Gierke @ Spring I/O 2018 - YouTube](https://www.youtube.com/watch?v=mQkf85S9UoQ&t=1s)
