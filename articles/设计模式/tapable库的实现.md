#### 发布订阅模式的高阶玩法

基本实现

```ts
class Tabable {
  subjects = []
  constructor () {

  }

  subscribe (subject) {
    this.subjects.push(subject)
  }

  publish (...agrs) {
    this.subjects.forEach(subject => {
      subject(...agrs)
    })
  }
}

const myTabable = new Tabable();

myTabable.subscribe((name) => {
  console.log(name, 'node')
});

myTabable.subscribe((name) => {
  console.log(name, 'webpack')
});

myTabable.publish('learn')
```

带了保险的钩子,如果前一个钩子返回值不是undefined，那么就不往下继续执行

```ts
class Tabable {
  subjects = []
  constructor () {

  }

  subscribe (subject) {
    this.subjects.push(subject)
  }

  publish (...args) {
    let i = 0;
    let ret;
    do {
      ret = this.subjects[i++](...args)
    } while (ret === undefined && i < this.subjects.length)
  }
}

const myTabable = new Tabable();

myTabable.subscribe((name) => {
  console.log(name, 'node');
  return 'node 还没学会'
});

myTabable.subscribe((name) => {
  console.log(name, 'webpack')
});

myTabable.subscribe((name) => {
  console.log(name, 'Vue3.0')
});

myTabable.publish('learn')
```

瀑布流的钩子， 前一个钩子的返回值是下一个钩子的参数, 重点是reduce函数的妙用

```ts
class Tabable {
  subjects = []
  constructor () {

  }

  subscribe (subject) {
    this.subjects.push(subject)
  }

  publish (...args) {
    this.subjects.reduce((ret, subject) => {
      ret = subject(ret);
      return ret
    }, ...args)
  }
}

const myTabable = new Tabable();

myTabable.subscribe((name) => {
  console.log(name, 'node');
  return 'node学完了'
});

myTabable.subscribe((name) => {
  console.log(name, 'webpack')
  return 'webapck学完了'
});

myTabable.subscribe((name) => {
  console.log(name, 'Vue3.0')
});

myTabable.publish('learn')
```