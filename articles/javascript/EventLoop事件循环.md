下面这段代码会造成栈溢出
```
function foo() {
	foo()
}
foo()
```
但是下面这段代码却不会，分析为什么
```
function foo() {
	setTimeout(foo)
}
foo()
```
function bar(color) {
	console.log(color)
	const nextColor = color === 'red' ? 'blue' : 'red'
	promise.resolve(nextColor).then((r) => {
		bar(r)
	})
}
