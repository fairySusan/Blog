### jsx语法的使用
1. 安装插件 npm install @vue/babel-plugin-jsx -D
2. 配置babel
```js
{
  "plugins": ["@vue/babel-plugin-jsx"]
}
```
3. 带有修饰符的事件的使用
```js
import {withModifiers, defineComponent} from 'vue';
const App = defineComponent({

  const clickHandle = () => {
    // do something...
  }

  setup() {
    return () => (
      <div onClick={withModifiers(clickHandle, ["self"])}></div>
    )
  }
})
```

4. 指令的使用
v-show 指令
```js
  setup() {
    return () => (<input v-show={visible} />)
  }
```

v-model 指令

```js
  setup() {
    return () => (<input v-model={val} />)
  }
```

v-if 指令

```js
  setup() {
    return () => (
      <div>
        { isShow && <div>hello world!</div> }
      </div>
    )
  }
```

默认 slots 插槽

```js
  setup() {
    return () => (
      <MyComponent>
        <span>xxxxx</span>
      </MyComponent>
    )
  }
```

./MyComponent.jsx

```js
  setup() {
    return () => (
      <div>
        {slot.default ? slot.defualt() : 'foo'}
      </div>
    )
  }
```

具名插槽

```js
  setup() {
    return () => (
      <MyComponent v-slots={{
        title: () => (<h1>my title</h1>)
      }}>
        <span>xxxxx</span>
      </MyComponent>
    )
  }
```

./MyComponent.jsx

```js
  setup() {
    return () => (
      <div>
        {slots.title ? slots.title() : 'default title'}
        {slots.default ? slots.defualt() : 'default value'}
      </div>
    )
  }
```

5. 在 TypeScript 中使用

tsconfig.json 文件

```js
{
  "compilerOptions": {
    "jsx": "preserve"
  }
}
```

### Vue3.x的v-model
vue2.0 的v-model用于自定义的组件时，v-model prop和事件名称：value和input
vue3.0 的v-model用于自定义的组件时，v-model prop和事件名称：modelValue和update:modelValue

vue3.0移除了 <input :value="value"/> v-bind指令及冒号写法，可使用<input v-model:value="value"/>代替

### Vue3.x的异步组件
vue2.0 时，异步组件的引入，`const asyncModal = () => import('./Modal.vue')`
vue3.0 时，异步组件的引入，
```js
// 不带选项的异步组件
const asyncModal = defineAsyncComponent(() => import('./Modal.vue'));
// 带选项的异步组件
const asyncModalWithOptions = defineAsyncComponent({
  loader: () => import('./Modal.vue'),
  delay: 200,
  timeout: 3000,
  errorComponent: ErrorComponent,
  loadingComponent: LoadingComponent
})
```

### 在jsx中使用动态异步组件
```js
import {KeepAlive, h, resolveComponent} from 'vue'
<KeepAlive>
  {
    h(resolveComponent(`Tab${activedTabId.value}`), {activedTabId: activedTabId})
  }
</KeepAlive>
```

### 如何挂载全局属性
vue2.0 `vue.property.$moment = moment`

vue3.x 
```js
const app = createApp(app);
app.config.globalProperties.$moment = moment
```

### 事件总线，推荐使用tiny-emitter库