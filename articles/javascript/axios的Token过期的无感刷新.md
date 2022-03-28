关于这个博文的实现一定要参照[简单的Promise实现](https://github.com/fairySusan/Blog/blob/master/articles/javascript/简单的Promise实现.md)和[EventLoop事件循环](https://github.com/fairySusan/Blog/blob/master/articles/javascript/EventLoop事件循环.md)来理解

```js
_axios.interceptors.response.use((response) => {
  // 状态码为2xx的情况
  return new Promise((resolve, reject) => {
    normalStatusHandle(response, resolve, reject)
  })
}, (error) => {
  const errResponse = error.response
  if (errResponse.status === 401) {
    // 将401的请求在刷新token后重新再次请求-无感刷新token
    return new Promise((resolve, reject) => {
      // 当requestFunctionQueue的length === 0 说明是刚初始化或者刚清过一次队列，此时再开始一次新的事件循环
      if (requestFunctionQueue.length === 0) {
        Promise.resolve().then(freshRequestQueue)
      }
      requestFunctionQueue.push(() => {
        _axios.request(errResponse.config).then((response) => {
          resolve(response)
        }, (err) => {
          reject(err)
        })
      })
    })
  } else {
    ElMessage({
      message: `状态码：${errResponse.status}`,
      grouping: true,
      type: 'error',
    })
    return Promise.reject({
      success: false,
      code: 0,
      data: errResponse.data,
      message: '系统繁忙，请稍后再试!'
    })
  }
});

function freshRequestQueue () {
  TokenHandler.getNewAccessToken().then(() => {
    let request
    while(request = requestFunctionQueue.shift()) {
     request()
    }
  })
}

function normalStatusHandle (response: AxiosRequestConfig, resolve: Function, reject: Function) {
  if (response.data.code === 0) {
    return resolve({
      success: true,
      ...response.data
    })
  } else if (response.data.code === 1) {
    ElMessage({
      message: response.data.message,
      grouping: true,
      type: 'error',
    })
    return reject({
      success: false,
      ...response.data
    })
  } else if (response.data.code === undefined) {
    return resolve({
      success: true,
      code: 0,
      data: response.data,
      message: ''
    })
  } else {
    ElMessage({
      message: response.data.message,
      grouping: true,
      type: 'error',
    })
  }
}
```