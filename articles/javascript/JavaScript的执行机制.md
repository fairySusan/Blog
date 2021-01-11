JS的执行分为两个阶段：1、创建阶段（Creation Phase）2、执行阶段（Execution Phase）
### 创建阶段
JS引擎在执行JS代码前，先创建了一个“全局执行上下文”，接着初始化“全局执行上下文”。在创建阶段总结JS干了以下事情：
* 创建了一个全局变量（window）
* 创建了一个叫this的变量，并指向全局变量
* 为变量和函数分配内存空间
* 安排变量的默认值为undefined（这里就是变量提升），并把函数声明放入内存中
做完上述的事情，JS引擎开始一行一行的执行代码，进入执行阶段

### 执行阶段
* 遇到 var name = ‘zhangsan’; 的语句，JS引擎把’zhangsan’安排到已在内存中的name变量
* 遇到函数调用，会生成一个“函数执行上下文”，JS引擎创建“函数执行上下文”会干一下的事情：
  * 创建一个argumants对象
  * 创建一个叫this的对象
  * 给变量和函数分配内存空间

### 总结
JS引擎创建执行上下文只有两个时候：一个是代码被解释的时候，创建全局执行上下文，一个是函数被调用的时候创建的函数执行上下文。JS引擎有一个执行上下文栈，显示全局执行上下文入栈，再是函数调用的时候形成的函数执行上下文压栈，函数里面又调用了函数新的函数执行上下文继续压栈，一个函数执行完，函数执行上下文就会出栈，销毁其内部数据。

所以了解了JS的执行原理可以思考一下事情：
1. 函数里this的指向
2. 闭包为什么会造成内存浪费

### 参考
[The Ultimate Guide to Hoisting, Scopes, and Closures in JavaScript](https://ui.dev/ultimate-guide-to-execution-contexts-hoisting-scopes-and-closures-in-javascript/?spm=ata.13261165.0.0.2d8e16798YR8lw)
