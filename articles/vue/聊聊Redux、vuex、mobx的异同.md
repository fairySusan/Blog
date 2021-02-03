### 状态管理器
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
```javascript
new Vue({
  data: {
    shareData: {
      counter: 0
    }
  }
})
```
所以我们需要一个工具，1、要记录下我们每一次对变量的更改，2、并且约束不能随意更改变量，更改行为要进行类

### Vuex
vuex提供了一个createStore函数来创建一个全局的store对象
```javascript
import { createStore } from 'vuex'
var store = createStore({
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
import { createStore } from 'vuex'
export default store = createStore({
  state: {
    counter: 0
  },
  action: {
    addCounter: function(state) {
      state.conuter++
    }
  },
  mutation: {},
})
```
在store挂载在Vue实例上,以便在组件里可以this.store.state.counter
```javascript
import store from './store.js'
import Vue from 'vue'
Vue.use(store)
```