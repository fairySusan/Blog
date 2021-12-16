```js
const p = new Promise((resolve, reject) => {
  resolve(1)
  console.log(2)
  new Error('error') // 不会改变promise对象的状态
})
p.then((r) => {
  console.log(r)
})
```

* resolve() 之后的代码可以继续执行，只是不能再改变promise对象的状态
* 改变promise对象的状态只能通过reslove(),reject(),Promise.reslove(),Promise.reject(), 
  直接抛出错误：`new Error('error')`并不能将fulfilled的状态改为rejected, 以下代码将输出'then'，不会输出'catch'
  ```js
    p.then((r) => {
      console.log(r)
      return new Error('error')
    }).then((r) => {
      console.log('then')
    }, (e) => {
      console.log('catch')
    })
  ```
* 但是下面的代码会输出'catch'，不会输出'then'
  ```js
    p.then((r) => {
      console.log(r)
      return Promise.reject()
    }).then((r) => {
      console.log('then')
    }, (e) => {
      console.log('catch')
    })
  ```
