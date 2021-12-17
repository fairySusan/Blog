```js
const p = new Promise((resolve, reject) => {
  resolve(1)
  console.log(2)
  throw new Error('error') // 不会改变promise对象的状态
})
p.then((r) => {
  console.log(r)
})
```

* resolve() 之后的代码可以继续执行，只是不能再改变promise对象的状态
* 改变promise对象的状态只能通过`reslove()`,`reject()`,`Promise.reslove()`,`Promise.reject()`, 或者 `throw new Error('error')`,或者代码执行出错。直接抛出错误：`throw new Error('error')`将pending的状态改为rejected, 以下代码将输出'catch'，不会输出'then'
  ```js
    p.then((r) => {
      console.log(r)
      return throw new Error('error')
    }).then((r) => {
      console.log('then')
    }, (e) => {
      console.log('catch')
    })
  ```
* 下面的代码也会输出'catch'，不会输出'then'
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

* promise的错误处理

  promise对象的错误具有“冒泡”性质，会一直向后传递，直到被捕获为止。当错误遇到一个catch就会被吞掉，停止往后传递了。
  若没有用catch来捕获这个错误，这个错误也会被promise吞掉，不会传递到外层代码，影响外层代码的执行，只会影响到promise内部代码的执行。

* promise.prototype.then()方法

  then 的回调方法里返回的是一个新的promise实例对象，默认返回值的情况下，返回的promise实例的状态取决于，then的回调函数体内有没有抛出错误。
  下面例子的then函数的回调函数的返回值相当于 `return Promise.reject()`
  ```js
  p.then(() => {
    throw new Error('error') 
  })
  ```

* promise.prototype.catch()方法

  catch方法的回调函数的返回值也是一个新的promise的实例对象,返回的promise实例的状态取决于，catch的回调函数体内有没有抛出错误。

  下面代码的p状态为rejected，并且会被catch捕获这个错误，但是catch函数的回调函数体内没有发生错误，所以catch的回调函数相当于`return Promise.resolve()`

  ```js
  const p = new Promise((resolve, reject) => {
    reject(0)
  })

  p.then(() => {
    //...
  }).catch((e) => {
    console.log(e)
  })
  ```

