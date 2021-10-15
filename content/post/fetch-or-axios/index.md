---
title: 'fetch and Axios'
date: 2021-10-14T15:41:59+09:00
description: Which should be used? Axios or fetch()
image: fetch-axios-cover.jpg
categories:
  - Frontend
  - Foundation
---

# Which should be used? Axios or fetch()

## Difference between Fetch and Axios for making http requests

### General

One the fundamental tasks of any web application is to communicate with servers through the http protocol. This can be easily achieved using Fetch or Axios.

- `fetch()` The Fetch API provides a `fetch()` method defined on the window object. It also provides a JavaScript interface for accessing and manipulating parts of the Http pipeline(requests and responses). The fetch method has **one mandatory argument** - URL of the resource to be fetched. The method retuirns a **Promise** that can be used to retrieve the response of the request.

```js
// fetch
fetch('api')
  .then((response) => {
    // Code for handling the response
  })
  .catch((error) => {
    // Code for handling the error
  });
```

- `Axios`: Axios is JavaScript library used to make HTTP requests from **nodejs or XMLHttpRequests** from the browser and it supports the Promise API that is native to JS ES6. It can be used intercept HTTP requests and respones and enables client-side protection against XSRF. It also has the ability to cancel requests.

```js
// Axios
axios
  .get('url')
  .then((response) => {
    // Code for handling the response
  })
  .catch((error) => {
    // Code for handling the error
  });
```

| Axios                                                                    | Fetch                                                                                                                                       |
| ------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------- |
| Axios has url in request object.                                         | Fetch has no url in request object.                                                                                                         |
| Axios is a stand-alone third party package that can be easily installed. | Fetch is built into most modern browsers; no installation is required as such.                                                              |
| Axios enjoys built-in XSRF protection.                                   | Fetch does not.                                                                                                                             |
| Axios uses the data property.                                            | Fetch uses the body property.                                                                                                               |
| Axios’ data contains the object.                                         | Fetch’s body has to be stringified.                                                                                                         |
| Axios request is ok when status is 200 and statusText is ‘OK’.           | Fetch request is ok when response object contains the ok property.                                                                          |
| Axios performs **automatic transforms of JSON data.**                    | Fetch is a two-step process when handling JSON data- first, to make the actual request; second, to call the .json() method on the response. |
| Axios allows cancelling request and request timeout.                     | Fetch does not.                                                                                                                             |
| Axios has the ability to intercept HTTP requests.                        | Fetch, by default, doesn’t provide a way to intercept requests.                                                                             |
| Axios has built-in support for download progress.                        | Fetch does not support upload progress.                                                                                                     |
| Axios has wide browser support.                                          | Fetch only supports Chrome 42+, Firefox 39+, Edge 14+, and Safari 10.1+ (This is known as Backward Compatibility).                          |

### Details

#### 1. Syntax Differences

Both Axios and Fetch API returns Promises when you make an HTTP request.

```js
// POST in axios
const options = {
  url: 'http://localhost:3000/api/home',
  method: 'POST',
  headers: {
    Accept: 'application/json',
    'Content-Type': 'application/json;charset=UTF-8',
  },
  data: {
    name: 'David',
    age: 45,
  },
};

axios(options).then((response) => {
  console.log(response.status);
});
```

With Axios, we can create a config object using specified parameters like baseUrl, params, headers, auth, responseType to send with the request. It will return a Promise that resolves with either a response object or an error object. Besides, the following can be found in the returned object from the Promise.

- `data`:- the actual response body
- `status`:- HTTP status of the call, like 200 or 404
- `statusText`:- HTTP status returned as a text message
- `headers`:- the server sends headers back
- `config`:- request configuration
- `request`:- XMLHttpRequest object

```js
// POST in fetch()
const url = 'http://localhost:3000/api/home';
const options = {
  method: 'POST',
  headers: {
    Accept: 'application/json',
    'Content-Type': 'application/json;charset=UTF-8',
  },
  body: JSON.stringify({
    name: 'David',
    age: 45,
  }),
};

// The url (path to the resource you want to fetch) argument is mandatory
fetch(url, options).then((response) => {
  console.log(response.status);
});
```

Similar to Axios, it returns a Promise that the response object can resolve. We can optionally pass options in the second argument in the `fetch()` method.

- **Key Differences:**
  - With Axios, the `data` is sent through the data property of the options, but Fetch API uses the `body` property.
  - Fetch response requires additional validation as it always returns a response object no matter whether it is successful or not.
  - The data in fetch() has been serialized to a String (Stringified).
  - The URL is provided to fetch() as an argument. But, in Axios, it is set in the options object.

#### 2. JSON Data Conversion

With **Fetch API**, handling JSON data is a 2 step process. First, you must make the request and then call the `.json()` function on the response since Fetch API sends data with the body property.

```js
fetch('localhost:3000/api') // first step
  .then((response) => response.json()) // second step
  .then((data) => {
    console.log(data);
  })
  .catch((error) => console.error(error));
```

With **Axios**, the data is sent through the data property of the options, and it automatically stringifies the data in the response.

```js
axios
  .get('localhost:3000/api/home')
  .then((response) => {
    console.log(response.data);
  })
  .catch((error) => {
    console.log(error);
  });
```

#### 3. Error Handling

Error handling with **Axios** is easy because bad responses (such as 404 or 500) will end up causing the Promise to be rejected by throwing an exception. Therefore, to handle 404 or 400 errors with Axios, you need to use the catch() block as shown below.

```js
axios
  .get('localhost:3000/api/home')
  .then((response) => {
    console.log('response', response);
  })
  .catch((error) => {
    if (error.response) {
      // The request was made and the server responded with a status code that falls out of the range of 200
      // Something like 4xx or 500
      console.log(error.response.data);
    } else if (error.request) {
      // The request was made but no response was received
      // `error.request` is an instance of XMLHttpRequest in the browser and an instance of http.ClientRequest in node.js
      console.log(error.request);
    } else {
      // Something happened in setting up the request that triggered an Error
      console.log('Error', error.message);
    }
    console.log(error.config);
  });
```

When using fetch(), you need to read the response object since bad responses are still resolved using the then() method. A Fetch API Promise will be **rejected only if the request cannot be completed in a scenario like a network failure**.

```js
fetch('localhost:3000/api/home')
  .then((response) => {
    if (!response.ok) {
      throw Error(response.statusText);
    }
    return response.json();
  })
  .then((data) => {
    console.log(data);
  })
  .catch((error) => console.error(error));
```

#### 4. Simultaneous Requests

Both **Axios** and Fetch API can handle multiple requests in parallel. Axios uses the `axios.all()` method that allows passing an array of requests. Then assign the properties of the response array to distinct variables using `axios.spread()` as shown here.

With **Fetch API**, you can use the built-in `Promise.all()` method to accomplish the same by passing all fetch requests to `Promise.all()` as an array. Next, you can use an async function to handle the response as follows.

```js
// axios
axios
  .all([
    axios.get('http://localhost:3000/api/home'),
    axios.get('http://localhost:3000/api/page'),
  ])
  .then(
    axios.spread((obj) => {
      // Both requests are now complete
      console.log(obj[0].data.login);
      console.log(obj[1].data.login);
    })
  );

// fetch
Promise.all([
  fetch('http://localhost:3000/api/home'),
  fetch('http://localhost:3000/api/page'),
])
  .then(async ([res1, res2]) => {
    const obj1 = await res1.json();
    const obj2 = await res2.json();
    console.log(obj1.login);
    console.log(obj2.login);
  })
  .catch((error) => {
    console.log(error);
  });
```

#### 5. Response timeout

If you make a request without defining a timeout it will cause the request to hang and slow down the application. So, we need to set a response timeout for HTTP requests.

```js
axios({
  method: 'post',
  url: 'localhost:3000/api/home',
  timeout: 3000, // 3 seconds timeout
  data: {
    name: 'David',
    age: 45,
  },
})
  .then((response) => {
    /* handle the response */
  })
  .catch((error) => console.error('timeout exceeded'));
```

> Axios sets the timeout to 0 by default. So, always remember to specify a timeout for each request. You may also use a request interceptor to set the request timeout automatically.

```js
// fetch

const controller = new AbortController();
const options = {
  method: 'POST',
  signal: controller.signal,
  body: JSON.stringify({
    name: 'David',
    age: 45,
  }),
};
const promise = fetch('http://localhost:3000/api/home', options);
const timeoutId = setTimeout(() => controller.abort(), 3000);

promise
  .then((response) => {
    /* handle the response */
  })
  .catch((error) => console.error('timeout exceeded'));
```

From the above example, you can see that `fetch()` response timeout functionality through `AbortController` interface. In addition, the read-only signal property of `AbortController` allows you to interact with or abort a request.

If the server doesn't respond within the specified time(3 seconds), `controller.abort()` is invoked, and the request is aborted.

#### 6. Intercepting Requests and Responses

> Fetch API doesn’t offer a way to intercept requests by default.

#### 7. Request Upload/Download Progress

If your HTTP request takes a significant time to complete, using a progress indicator will surely help to improve the user experience.
If you use Axios, you can easily use the **Axios Progress Bar module** to implement a nice progress indicator.

> But Fetch API doesn’t have in-built support for progress bars.

---

## Conclusion

This comparison demonstrates that Axios keeps the code minimal for applications that require efficient error handling or HTTP interceptions. it supports almost all modern browsers and NodeJS environments.

On the other hand, Fetch API isn’t far off either as a native method supported by all the major browsers (it doesn’t support IE).

However, we can not stick to a single aspect in choosing the best option. Instead, you need to evaluate all these features and decide what’s best for your project based on its requirements.

## Reference

[Axios or fetch(): Which should you use? - LogRocket Blog](https://blog.logrocket.com/axios-or-fetch-api/)

[Performing HTTP Requests: Fetch Vs Axios | by Piumi Liyana Gunawardhana](https://blog.bitsrc.io/performing-http-requests-fetch-vs-axios-b62b44fed10d)
