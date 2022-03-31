### `node`与浏览器的异同(此处的`node`我们指的是js的运行环境，Node.js是`在node里运行的js`的一个新的名字)
  * `node`和浏览器都使用`JavaScript`作为其编程语言
  * `node`使用的是V8引擎来解释JS
  * 在浏览器中有`DOM`，`Web平台的API`（如浏览器的`localStorage`， `window`），但是在`node`中没有， 但是`node`中也有浏览器中没有的文件访问系统
  * node使用`CommomJS模块系统`，浏览器使用`ES Modules标准`

### 如何运行node脚本
  * 直接在Shell里面`node app.js`，这告诉shell使用node来运行你的脚本
  * 或者使用“shebang”行将“用`node`来运行你的脚本”告诉shell
  ```JS
  #!/usr/bin/env node
  ```
### 从node中读取环境变量
  * `Node.js`的`process`核心模块提供了`env`属性，该属性承载了在启动进程时设置的所有环境变量

### Node.js从命令行接收参数
 * 在shell中
  ```
  node app.js joe // 1
  or
  node app.js name = joe // 2
  ```
   - 获取参数值的方法是使用`Node.js`中内置的`process.argv`,`argv`属性是一个包含所有命令行调用参数的数组。
   - 参数数组的第一个元素是`node`命令的完整路径，第二个参数是正被执行的文件的完整路径，所有其他的参数从第三个位置开始。
   - 可以排除前两个参数得到传入的参数数组`const args = process.argv.slice(2)`
   - 对于以上述1方式传入的参数可以直接`args[0]`来获取，对于2方式传入的参数`args[0]`获取的是`name=joe`，需要使用`minimist库`来解析
    ```js
    const args = require('minimist')(process.argv.slice(2))
    args['name'] // joe
    ```
    但是在传入参数的时候需要在每个参数名称之前加上双破折号`node app.js --name=joe`
### 使用Node.js输出到命令行
  * 基础输出`console.log()`
  * 清空控制台`console.clear()`
  * 元素计数`console.count()`
  * 打印函数调用堆栈信息`console.trace()`
  * 计算函数运行所需要的时间`console.time(); console.timeEnd()`
  * 为输出着色`console.log('\x1b[33m%s\x1b[0m', '你好')`这回输出黄色的'你好',一般使用`Chalk`库，还有助于其他样式（字体加粗、斜体、带下划线）
  * 创建进度条`Progess`可用于在控制台创建进度条

### Node.js在终端中进行交互
  * 从版本7开始，`Node.js`提供了`readline模块`来执行交互操作
    ```js
    const readline = require('readline').createInterface({
      input: process.stdin,
      output: process.stdout
    })

    readline.question(`你叫什么名字?`, name => {
      console.log(`你好 ${name}!`)
      readline.close()
    })
    ```
    以上在终端中询问用户名，当输入了文本并且按下了回车，就会发送"你好，xxx"
    常见的封装好的包有`inquirer`,它可以询问多项选择，展示单选按钮，确认等

### 

