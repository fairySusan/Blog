### 函数作用域
首先什么是作用域？
作用域就是一个区域，在这个区域之外无法获取这个区域的变量。

javascript采取的是`静态作用域`，静态作用域的特点就是：<b>函数的作用域在函数定义时就确定了。</b>

举个🌰
```javascript
var a = 1

function foo () {
  var a = 2
  console.log(a)
}

foo()
```
显而易见打印出2，因为foo的作用域在foo定义的时候确定，此作用域中有一个a变量，当js在foo的作用域中执行`console.log(a)`时，会去当前作用域查找a变量。
所以记住这句话：<b>函数的作用域在函数定义时就确定了，而不是执行的时候。</b>

### 作用域链

函数在定义的时候不仅确定了自己的作用域，还确定了自己的父作用域。它的父作用域就是函数定义时所在的那个作用域。

举个🌰

```javascript
var a = "全局作用域";
function foo(){
    var a= "foo作用域";
    function bar (){
      console.log(a)
    }
    return bar()
}
foo();
```

打印出"foo作用域"。js执行`console.log(a)`时会去bar函数作用域查找变量a，但是bar函数作用域里并没有变量a，接着js引擎会去bar的父作用域查找变量a。
bar在foo作用域里定义的，所以foo作用域便是bar函数的父作用域，最后在foo作用域里找到了a变量。

再举个🌰

```javascript
var a = "全局作用域";
function foo(){
    var a= "foo作用域";
    function bar (){
      console.log(a)
    }
    return bar
}
foo()();
```
注意和上面代码的区别。
依然打印出"foo作用域"。

这两个例子都在证明，函数作用域与函数在哪里调用无关，由函数在哪里定义决定！





