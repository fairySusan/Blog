## typeof 操作符
typeof 返回的值有： undefined、number、string、boolean、object、function、symbol
typeof null 返回 object，因为null是一个空对象的指针

## Undefined
`var a;`
声明一个变量不初始化的话，变量的值为undefined
undefined转成boolean值为false

## Null
null是一个空对象的指针。null转成boolean值为false
`console.log(null == undefined) // true`

## Boolean
Boolean类型只有两个值：true、false 并且区分大小写，所有类型的值都可以转换成一个Boolean值。
转换方法：隐式转换（参见后文有关隐式转换的内容）、Boolean()
|数据类型|转换为true的值|转换为false的值|
|------ |------------|-------------|
|Number|任何非0数，包括无穷大|0/NaN|
|String| 任何非空字符串|''|
|Object|任何对象|null|
|undefined|--|undefined|


## Number
### 只总结NaN的特性：
* `typeof NaN // "number" `
* 在其他编程语言中任何数值除以0都会报错。但在JS中，会返回NaN但是不影响代码的执行。
* 任何涉及NaN的计算都会返回NaN，比如NaN/10
* NaN与任何值都不相等，包括NaN本身， NaN == NaN // false
* isNaN()用来判断是否“不是数值”
  * `isNaN(NaN) // true`
  * `isNaN(10) // false`
  * `isNaN('10') // false`
  * `isNaN('number') // true`
  * `isNaN(true) // false`
### 数值转换
有3个函数可以把非数值转换为数值： Number() 、parseInt()、 parseFloat()
Number()参数可以为任何数据类型, parseInt()、 parseFloat()参数为字符串类型
Number()函数的转换规则：
* Number(true) // 1
* Number(false) // 0
* Number(null) // 0
* Number(undefined) // NaN
* 字符串
  * Number('123') // 123
  * Number('0123') // 123 (注意：前导的零被忽略了)
  * Number('+123') // 123
  * Number('-123') // -123
  * Number('1.23') // 1.23 (注意：前导的零被忽略了)
  * Number('0xf') // 15 (转换为相同值的十进制)
  * Number('') // 0
  * Number('string') // NaN
一元加操作符跟Number()函数相同
由于Number函数在转换字符串时不太合理，就产生了parseInt()函数
* parseInt('') // NaN (注意：这里跟Number('')不一样)
* parseInt('123') // 123
* parseInt('123blue') // 123
* parseInt('22.2') // 22
* parseInt('0xA') // 10 (解析十六进制，输出十进制)
* parseInt('070') // 56 (解析八进制，输出十进制)

parseFloat()函数
* parseFloat('123blue') // 123
* parseFloat('22.5') // 22.5
* parseFloat('22.5.6') //22.5
* parseFloat('0908.1') // 908.1

### String
String类型表示由零个或多个16位unicode字符组成的字符序列，称为字符串
String数据类型包含一些特殊的字符字面量：
|字面量|含义|
|-----|----|
|\n|换行|
|\t|制表|
|\b|空格|
|\n|回车|
|\\\|斜杠|
|\\'|单引号|
|\\"|双引号|

将其他值转换为字符串有两种办法：toString()、String()
toString()
* 数值、布尔、对象、字符串值都有toString()方法，但是null和undefined没有toString()方法
* `var a = {name: 'xiaoming'}; a.toString() // "[Object Object]"`
* true.toString() // "true"
* 1.22.toString() // "1.22"
* null.toString() // 报错
* undefined.toString() // 报错

String()
* 如果值有toString()方法则调用该方法返回对应的值
* String(null) // "null"
* String(undefined) // "undefined"

<b>要把某个值转换为字符串，可以使用加号操作符把它和一个""加在一起</b>

### 基本类型值获取不存在的属性
* `'this a string'.redColor // 'undefined'`
* `true.redColor // 'undefined'`
* `10.redColor // 报错`
* `undefined.redColor // 报错`
* `null.redColor // 报错`
