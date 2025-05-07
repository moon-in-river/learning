# 异常处理

http 异常状态码可以通过返回值的 ok、status 来判断。

```js
fetch(url)
  .then((res) => {
    if (res.ok) {
      return res.json();
    } else {
      return Promise.reject({
        status: res.status,
        statusText: res.statusText,
      });
    }
  })
  .then((data) => {
    // 处理业务
    console.log(data);
  })
  .catch((error) => {
    console.error(error);
  });

fetch(url)
  .then((res) => {
    return res.json().then((json) => {
      if (res.ok) {
        return json;
      } else {
        return Promise.reject(json);
      }
    });
  })
  .then((data) => {
    // 处理业务
    console.log(data);
  })
  .catch((error) => {
    console.error(error);
  });
```

# 不同响应类型

不同响应类型用不同的方法。

- application/json 用 json 方法。
- text/html 用 text 方法。

```js
fetch(url).then((res) => {
  const contentType = res.headers.get("content-type");
  if (contentType.includes("application/json")) {
    return res.json();
  } else if (contentType.includes("text/html")) {
    return res.text();
  } else {
    // other type
  }
});
```
