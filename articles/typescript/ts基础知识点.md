#### 索引类型查询操作符

`keyof T`,对于任何类型T, keyof T 的结果为T上已知的公共属性名的联合。
例如：
```ts
interface Person {
  name:string;
  age: number;
}

let personProps: keyof Person; // 'name' | 'age'
```

#### 索引访问操作符

`T[K]` 例如：
```ts
let name: Person['name'] // let name:string
```

再例如：
```ts
type MyPick<T, K extends keyof T> = {
  [P in K]: T[K]
}

```

#### 映射类型

映射类型：ts提供了从旧类型中创建新类型的方式

一个常见的任务就是将已知类型的每个属性都变为可选
```ts
type Partial<T> = {
  [P in keyof T]?: T[P]
}

```
```ts
type ReadOnly<T> = {
  readonly [P in keyof T]: T[P]
}
```




