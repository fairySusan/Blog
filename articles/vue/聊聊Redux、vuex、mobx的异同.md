## 状态管理器
-------------------------
回顾我们初学html，javascript的时候，没有vue，react这些框架，最原始的写法在html里script标签引入js文件
```html
<html>
  <body></body>
  <script src="./main.js"></script>
  <script src="./home.js"></script>
  <script src="./foo.js"></script>
</html>
```
假如有一个全局变量a，以上三个js文件都需要用到，那a在三个文件里都可能被修改，并且我们很难去定位**1、在哪个时刻 2、在哪个位置 3、进行里怎样的修改**

后来我们有了vue、react这种框架，我们有了组件这种概念，但是组件之间的数据共享操作起来太麻烦。
父组件的数据需要通过props传递给子组件，每个子组件都要写一遍props来接收。而兄弟组件之间需要事件传递数据，或者把数据传给父组件，父组件传给另一个子组件。
项目庞大后，代码变得复杂，难以维护。

所以vue有一个全局store的概念：
在项目的main.js里，实例化Vue的时候在data属性里定义需要的全局变量，在组件里通过this.$root.$data.shareData去访问,但是这不跟我们原始html，js的效果是一样的吗？
依然不知道**1、在哪个时刻 2、在哪个位置 3、进行里怎样的修改**

main.js
```javascript
new Vue({
  data: {
    shareData: {
      counter: 0
    }
  }
}).$mount('#app')
```
所以我们需要一个工具:
* 1、要记录下我们每一次对变量的更改
* 2、并且约束不能随意更改变量，更改行为要进行命名
* 3、在MVVM框架流行的今天，当然也要更改变量自动触发绑定的视图的更新

## Vuex
-------------------------
```javascript
import Vuex from 'vuex'
export const store = new Vuex.store({
  state: {},
  action: {},
  mutation: {},
  modules: {}
})
```
vuex里的重要概念有state、action、mutation
* state：一个对象，就是全局变量的集合
* action：方法的集合，要对state进行怎么样的修改
* mutation：跟action一样，但是只能写同步代码

🌰
```javascript
import Vuex from 'vuex'
import Vue from 'vue'
Vue.use(Vuex)
export const store = new Vuex.store({
  state: {
    counter: 0
  },
  action: {
    addCounter: function(state) {
      state.conuter++
    },
    subCounter: function(state) {
      state.counter--
    }
  },
  mutation: {},
})
```
在store挂载在Vue实例上,以便在组件里可以this.$store.state.counter

main.js
```javascript
import { store } from './store.js'
import Vue from 'vue'
new Vue({
  store
}).$mount('#app')
```
组件里面
```javascript
mounted () {
  // 读取state
  this.$store.state.counter
  // 改变state
  this.$store.dispatch('addCounter')
}
```
实现原理：[简单实现一个vuex](https://github.com/fairySusan/vuex-demo)

## Redux
-------------------------
redux里的概念有：state、action、reducer
前文说了action代表了你要对state作出什么样的改变

对比vuex，redux所有对state的改变的逻辑都在reducer里面,用switch case来匹配要对state做何种改变
而action就是{type: 'addCounter'}这样的一个对象而已。

counter.js
```typescript
// reducer
export function counter(state = 0, action: {type: string} | {type: string; num: number}) {
  switch(action.type) {
    case 'addCounter':
      return state++
    case 'multiplyCounter':
      return state * num
  }
}
```

redux有一个createStore函数。用来注册store

store.js
```javascript
import {createStore} from 'redux'
import {counter} from 'counter.js'

export const store = createStore(counter)
```

组件里
```javascript
import {store} from './store.js'
import {multiplyCounter} from './action.js'
// 读取state
store.getState().counter
// 改变state
store.dispatch({type: 'addCounter'})
// 如果需要传参数
store.dispatch({type: 'multiplyCounter', num: 3})
// 一般会把action写成一个函数，返回一个带type属性的对象, action.js如下
store.dispatch(multiplyCounter(3))
```
action.js
```javascript
export const multiplyCounter = (num) => { 
  return {type: 'multiplyCounter', num}
}
```
## react-redux
vuex是专门给vue写的一个状态管理工具，跟vue是官配。而redux跟react没有任何关系，redux可以在任何框架的项目中使用，甚至vue。
react-redux就是连接redux和react的一个工具，便于可以在react组件中props.counter、props.dispatch()...


## mobx
