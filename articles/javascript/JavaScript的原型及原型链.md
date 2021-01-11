javascript不是面向对象的语言，那么是怎么实现继承的呢？javascript则依靠原型和原型链来实现继承。
### prototype
javascript每个<b>函数</b>都有一个prototype的属性，它是一个对象
```
function Person {}
var xiaoming = new Person()
```
上面的例子就用Person构造函数生成了一个实例xiaoming（js没有对于构造函数特定的写法， 什么函数都可以作为构造函数，约定俗成构造函数函数名首字母大写。）
我们在构造函数的prototype对象上添加两个属性
```
Person.prototype.sex = 'boy'
console.log(Person.prototype) // 可打印出来看看prototype里面有什么
console.log(xiaoming.sex) // 'boy'
```
可见xiaoming这个实例拿到了`Person.prototype`里的属性。因为js<b>每个对象（除了null）</b>都有一个隐式属性__proto__,
每个对象的__proto__属性都是指向创建自己的构造函数的prototype对象，上面的例子`xiaoming.__proto__ `指向`Person.prototype`,所以`xiaoming.__proto__.sex`可以获取到（__proto__可以省略不写，即为xiaoming.sex）

如果给xiaoming自己添加了一个属性sex，读取的时候会读取自身的sex属性
```
xiaoming.sex = 'girl' // 注意，执行这一步只是给xiaoming自己添加了一个sex属性，并没有更改Person.prototype.sex属性哦
console.log(xiaoming.sex) // 'girl'
console.log(Person.prototype.sex) // 'boy'
```

`Person.prototype`对象里还存在一个constructor对象，它指向Person，这样就形成里一个闭环

### 原型链
现在我们让一个函数来继承Person
```
function Man () {}
Man.prototype = new Person()
Man.prototype.constructor = Man
console.log(Man.prototype.sex) // 'boy'

var zhangwei = new Man()
console.log(zhangwei.sex) // 'boy'
```
分析一下：首先`zhangwei.__proto__` 指向Man.prototype，重点`Man.prototype`也是一个对象，并且是Person构造函数new出来的，所有`Man.prototype.__proto__`指向`Person.prototype`,由此可得zhangwei.sex拿取的是`Person.prototype.sex`。
让我们继续往下分析下去：
Person.prototype也是一个对象，它是定义了Person构造函数就有的，所以它是new Object()出来的，由此可得`Person.prototype.__proto__`指向`Object.prototype`,那`Object.prototype.__proto__`指向谁？答案只有一个，指向null，原型链的终点。所以前文说了null没有隐式属性__proto__
