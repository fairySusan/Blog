```js
class Dep {
  deps = [];

  constructor () {}

  addDeps () {
    this.deps.push(Dep.currentEffect)
  }

  notify () {
    this.deps.forEach(dep => {
      dep()
    })
  }
}

Dep.currentEffect = null;
```

ref----的实现
```js
class Ref {
  _value;

  constructor (value) {
    this._value = value
  }

  get value () {
    DepIns.addDeps();
    return this._value;
  }

  set value (value) {
    this._value = value;
    DepIns.notify();
  }
}

function ref (value) {
  return new Ref(value)
}

function watchEffect (cb) {
  Dep.currentEffect = cb;
  cb();
  Dep.currentEffect = null
}

const DepIns = new Dep()
```

ref---实践
```js
const hello = ref('hello');

watchEffect(() => {
  console.log(hello.value)
});

hello.value = 'hello world'
```

reative---实现
```js
const targetMap = new Map();

function reactive (obj) {
  const proxy = new Proxy(obj, {
    set: (target, key, value) => {
      const depMap = targetMap.get(target)
      const dep = depMap.get(key);
      dep.notify();
      return value
    },
    get: (target, key) => {
      let depsMap = targetMap.get(target)
      if (!depsMap) {
        depsMap = new Map();
        targetMap.set(target, depsMap)
      }

      let dep = depsMap.get(key);

      if (!dep) {
        dep = new Dep();
        depsMap.set(key, dep)
      }

      dep.addDeps()

      return Reflect.get(target, key)
    }
  })

  return proxy
}
```

reactive---实践
```js
const user = reactive({
  age: 10
})

watchEffect(() => {
  console.log(user.age)
});

user.age = 20
```

