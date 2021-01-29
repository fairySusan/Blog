### 任务队列
js是单线程的，只有一个主线程。
但是像ajax请求、setTimeout、setInterval都是耗时很长的事件
```javascript
setTimeout(function(){
	console.log(1)
}, 1000)
console.log(2)
```
按照单线程的执行，先等待1s后输出1，再输出2。但其实是先输出1，再输出2。

js是怎么实现这种异步执行的呢？
js引擎里有一个任务队列，专门用来存放，异步操作的回调函数。
以上面最简单的例子：
主线程先执行setTimeout，将其回调函数放入任务队列中，接着执行console.log(2)语句

![事件循环机制图](/eventloop1.jpg)

等到主线程空了，主线程再去从任务队列里获取回调函数执行。

![事件循环机制图](/eventloop2.jpg)

这就解释了下面的代码为什么依然是异步执行
```javascript
setTimeout(function(){
	console.log(1)
}, 0)
console.log(2)
```

### 微任务和宏任务
上面我们打好了任务队列的基础，但是不是所有的异步操作的回调函数都是顺序执行的。js引擎把任务分为微任务和宏任务。
宏任务：
* script(整体代码)
* setTimeout
* setInterval
* I/O
* setImmediate

微任务：
* Promise.then
* MutaionObserver（node）
* process.nextTick （node）

微任务和宏任务的异步执行是有先后顺序的，微任务的回调函数会放入微任务队列，宏任务的会调函数会放入宏任务队列。
执行顺序：主线程-> 微任务队列 -> 宏任务队列。
举个例子：
```javascript
setTimeout(function() {
	console.log(1)
}, 2000)

Promise.resolve(2).then(() => {
	console.log(2)
})

console.log(3)
```

结果：3 2 1

首先主线程先执行setTimeout，将其回调函数放入宏任务队列，再执行Promise.resolve将其回调函数放入微任务队列，最后直接执行console.log(3)输出3。
console.log(3)执行完毕后，主线程空了，然后去微任务队列里拿取`() => {console.log(2)}`来执行，输出2。主线程再去微任务队列里拿取任务，微任务已经为空。
主线程就去宏任务队列拿回调函数，输出1

![事件循环机制图](/eventloop3.jpg)

![事件循环机制图](/eventloop4.jpg)

![事件循环机制图](/eventloop5.jpg)


## 思考
根据[JavaScript的执行机制](/JavaScript的执行机制.md)，函数的每一次调用都会生成一个`函数执行上下文`,然后入栈。
函数执行完，`函数执行上下文`出栈。当入栈的函数太多便会造成栈溢出。

所以下面这段代码会造成栈溢出
```javascript
function foo() {
	foo()
}
foo()
```
不断的调用foo，不断的生成执行上下文，不断的入栈，造成栈溢出

但是下面这段代码却不会，分析为什么
```
function foo() {
	setTimeout(foo)
}
foo()
```
