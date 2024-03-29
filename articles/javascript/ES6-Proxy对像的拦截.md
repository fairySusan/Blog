Proxy 是一个构造函数。Proxy编程属于对编程语言编程，属于'元编程'。

*`Proxy`可以理解为在对象的操作之前架设了一层‘拦截’，外界对该对象的访问，都必须先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写*

就是“代理模式”。

Proxy构造器传入两个参数。第一个参数是目标对象，目标对象包括：对象、数组、函数...,第二个参数是拦截方法的集合对象。

```js
const dinner = {
  meal: 'tacos'
}

const handler = {
  get(target, prop, receiver) {
    console.log('读操作-拦截器');
    return target[prop]
  }
}
const proxyer = new Proxy(dinner, handler)

console.log(proxyer.meal)
```

如果上面的handler是一个空对象，那么访问proxyer.meal等同于访问dinner.meal。

Proxy可以对函数的调用进行拦截

```js

const sum = function (x, y) {
  return x + y
}
const handler = {
  apply: function (target, thisBings, args) {
    return args[0]
  },
  construct: function (target, args) {
    return args[1]
  }
}
const proxyer = new Proxy(sum, handler);

proxyer(1,2) //1

new proxyer(1,2) // 2

```

#### Proxy支持的拦截操作一共13种

1. `get(target,propKey,receiver)`: 拦截对象属性的读取，比如`proxy.foo`和`proxy['foo']`

2. `set(target,propKey,value,receiver)`: 拦截对象属性的设置，比如`proxy.foo='v'`和`proxy['foo'] = v`，返回一个布尔值

3. `has(target,propKey)`: 拦截`propKey in proxy`的操作，返回一个布尔值

4. `deleteProperty(target, propKey)`:拦截`delete proxy[proxyKey]`的操作,返回一个布尔值

5. `ownKeys(target)`：拦截`Object.getOwnPropertyNames(proxy)`、`Object.getOwnPropertySymbols(proxy)`、`Object.keys(proxy)`、`for...in`循环，返回一个数组。该方法返回目标对象所有自身的属性的属性名，而`Object.keys()`的返回结果仅包括目标对象自身的可遍历属性。

6. `getOwnPropertyDescriptor(target, propKey)`：拦截`Object.getOwnPropertyDescriptor(proxy, propKey)`，返回属性的描述对象。

7. `defineProperty(target, propKey, propDesc)`：拦截`Object.defineProperty(proxy, propKey, propDesc）`、`Object.defineProperties(proxy, propDescs)`，返回一个布尔值。

8. `preventExtensions(target)`：拦截`Object.preventExtensions(proxy)`，返回一个布尔值。

9. `getPrototypeOf(target)`：拦截`Object.getPrototypeOf(proxy)`，返回一个对象。

10. `isExtensible(target)`：拦截`Object.isExtensible(proxy)`，返回一个布尔值。

11. `setPrototypeOf(target, proto)`：拦截`Object.setPrototypeOf(proxy, proto)`，返回一个布尔值。如果目标对象是函数，那么还有两种额外操作可以拦截。

12. `apply(target, object, args)`：拦截 `Proxy` 实例作为函数调用的操作，比如`proxy(...args)`、`proxy.call(object, ...args)`、`proxy.apply(...)`

13. `construct(target, args)`：拦截 `Proxy` 实例作为构造函数调用的操作，比如`new proxy(...args)`。

### `Proxy`和`Object.defineProperty()`的对比
  * `Proxy`可以监听到对象添加新的属性,而`Object.defineProperty()`不能
  * `Proxy`可以监听到对象属性的删除，用`deleteProperty()`，后者不能
  * `Proxy`可以监听到对象的for in操作，in操作，后者不能

### 当`child`对象里没有`name`属性的时候，去读取`name`属性那么会去child的原型上拿`getter`方法，同理`set`的时候也是的回去原型上拿原型的`setter`方法
```JS
const child = {
}
const parent = {
  name: 'taorui'
}
const proxyChild = new Proxy(child, {
  set(target, key, value, receiver) {
    console.log('child set')
    return Reflect.set(target,key,value, receiver)
  },
  get(target, key, receiver) {
    console.log('child get')
    return Reflect.get(target,key, receiver)
  }
})

const proxyParent = new Proxy(parent, {
  set(target, key, value, receiver) {
    console.log('parent set')
    return Reflect.set(target,key,value, receiver)
  },
  get(target, key, receiver) {
    console.log('parent get')
    return Reflect.get(target,key, receiver)
  }
})

Object.setPrototypeOf(proxyChild, proxyParent)

console.log(proxyChild.name)

proxyChild.name = 'lijianguo'
```


