首先我们先来看看promise的使用

```javascript
const p = new Promise((resolve, reject) => {
  if (/*异步成功*/) {
    resolve(data)
  } else {
    reject()
  }
})

p.then(() => {
  /*异步成功后的回调函数，也就是resolve函数*/
  return 'return value' // 或者 return new Promise()
}, () => {
  /*异步失败后的回调函数，也就是reject函数*/
})
```

```javascript
class GPromise {
    constructor(executor) {
        this._promiseStatus = GPromise.PENDING;
        this._promiseValue;
        this.resolvedQueue = [];
        this.rejectedQueue = [];
        this.execute(executor);
    }
    execute(executor) {
        if(typeof executor !== 'function' ) {
            throw new Error('GPromise resolver ${executor} is not a function')
        }
        try {
            executor( this.resolve.bind(this), this.reject.bind(this));
        } catch(e) {
            this._promiseStatus = GPromise.REJECTED;
            this._promiseValue = e;
        }
    }
    then(onfulfilled, onrejected) {
        return this.asyncCallBack(onfulfilled, onrejected)
    }
    asyncCallBack(onfulfilled, onrejected) {
        // then函数会返回一个promise实例，并且会resolve(回调函数的返回值也就是returnValue)，作为下一个then函数回调函数的参数
        if (this._promiseStatus === GPromise.FULFILLED) {
            return new GPromise((resolve, reject) => {
                setTimeout(() => {
                    const returnValue = onfulfilled(this._promiseValue);
                    if (returnValue instanceof GPromise) {
                        returnValue.then(resolve, reject)
                    } else {
                        resolve(returnValue)
                    }
                },0);
            })
        }
        if (this._promiseStatus === GPromise.REJECTED) {
            return new GPromise((resolve, reject) => {
                setTimeout(() => {
                    const returnValue = onrejected(this._promiseValue);
                    if (returnValue instanceof GPromise) {
                        returnValue.then(resolve, reject)
                    } else {
                        reject(returnValue)
                    }
                }, 0)
            })
        }
        //处理then的链式调用，链式调用的then的回调函数放到一个任务队列里等待this._promiseStatus改变里之后再执行
        if (this._promiseStatus === GPromise.PENDING) {
            return new GPromise((resolve, reject) => {
                this.resolvedQueue.push(() => {
                    const returnValue = onfulfilled(this._promiseValue);
                    if (returnValue instanceof GPromise) {
                        returnValue.then(resolve, reject)
                    } else {
                        resolve(returnValue)
                    }
                });

                this.rejectedQueue.push(() => {
                    const returnValue = onrejected(this._promiseValue);
                    if (returnValue instanceof GPromise) {
                        returnValue.then(resolve, reject)
                    } else {
                        reject(returnValue)
                    }
                })
            })
        }
    }
    resolve(data) {
        if (this._promiseStatus !== GPromise.PENDING) return;
        this._promiseStatus = GPromise.FULFILLED;
        this._promiseValue = data;
        for(let i = 0; i < this.resolvedQueue.length; i++) {
            this.resolvedQueue[i](data)
        }
    }
    reject(e) {
        if(this._promiseStatus !== GPromise.PENDING) return;
        this._promiseStatus = GPromise.REJECTED;
        this._promiseValue = e;
        for(let i = 0; i < this.rejectedQueue.length; i++) {
            this.rejectedQueue[i](e)
        }
    }
}
GPromise.PENDING = 'pending';
GPromise.FULFILLED = 'fulfilled';
GPromise.REJECTED = 'rejected';
```