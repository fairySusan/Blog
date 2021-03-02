每次我们创建新的项目都会使用`vue init webpack myProject`来生成一个项目框架。
公司里每次的项目其实都是大差不差的目录结构，比如utils文件夹,store的配置,UI框架的配置，axios的全局配置，每次新建一个项目都需要去配置一遍。

那我们可以写一个符合自己公司业务的脚手架工具，在初始化项目的时候就把上述依赖及配置都自动初始化好呢？

肯定可以啦！

接下来我们就来写一个自己的脚手架工具

首先新建一个文件夹tr-cli

`cd tr-cli`

`npm init`

在tr-cli项目根目录新建一个bin文件夹，bin文件夹里新建tr.js文件。

ok，现在我们来下载一些需要用到的npm包

### commander

用来定义指令和执行指令。比如vue init这个命令，就用这个工具来定义

### inquirer

交互式命令工具，vue init后会有几个问题，比如项目名？，使用js还是typescrip? ...

### chalk

用来修改控制台的输出内容的样式，比如颜色。

### ora

一个加载的动画，在下载模版的时候，需要用到这个，让用户知道正在下载模版。

### download-git-repo

看名字很明显了，这是用来下载远程模板的

npm install 上面的依赖

然后我们在bin目录下的tr.js文件里写入

```javascript
#!/usr/bin/env node
const program = require('commander')

program
  .version(require('../package.json').version)
  .usage('<command> [options]')
  .command('add', 'add a new template')
  .command('remove', 'remove a template')
  .command('list', 'list all the template')
  .command('init', 'generate a new project from a template')

// 解析命令行参数
program.parse(process.argv)
```

vue-cli就可以直接使用vue这个命令，我们怎么直接使用tr这个命令呢？

这里需要在pacakage.json里去配置bin属性

```json
 {
   "name": "tr-cli",
  "version": "1.0.0",
  "description": "vue 脚手架工具",
  "main": "index.js",
  "bin": {
    "tr": "bin/tr.js",
  },
  // other...
}
```

然后`npm link`（npm link 就是把命令挂载到全局的意思）

现在我们执行 `tr` 试试看。看看控制台的输出

ok 现在我们继续在bin目录下新建add.js init.js list.js remove.js

在项目的根目录下新建template.json, 现在只写一个 `{}`

然后我们来修改一下package.json的bin里的内容

```json
 {
  "name": "tr-cli",
  "version": "1.0.0",
  "description": "vue 脚手架工具",
  "main": "index.js",
  "bin": {
    "tr": "bin/tr.js",
    "tr-init": "bin/init.js",
    "tr-add": "bin/add.js",
    "tr-remove": "bin/remove.js",
    "tr-list": "bin/list.js"
  },
  // other...
}
```

然后 `npm unlink` 再次`npm link`

接下来就是在每个文件里写入代码了

### add.js

```javascript
#!/usr/bin/env node

// 交互式命令行
const inquirer = require("inquirer");

// 修改控制台字符串的样式
const chalk = require("chalk")

// node内置文件模块
const fs = require('fs')

// 读取根目录下的template.json
const tplObj = require(`${__dirname}/../template.json`)

// 自定义交互式的问题和简单的校验
let question = [
  {
    name: "name",
    type: "input",
    message: "请输入模版名称",
    validate (val) {
      if (val === '') {
        return 'Name is required!'
      } else if (tplObj[val]) {
        return 'template has already existed!'
      } else {
        return true
      }
    }
  },
  {
    name: 'url',
    type: 'input',
    message: '请输入模版地址',
    validate (val) {
      if (val === '') return 'The url is required!' 
      return true
    }
  }
]

inquirer.prompt(question).then(answers => {
  // answers 就是用户输入的内容，是个对象
  let { name, url} = answers
  // 过滤 unicode 字符
  tplObj[name] = url.replace(/[\u0000-\u0019]/g, '')
  // 把模板信息写入 template.json 文件中
  fs.writeFile(`${__dirname}/../template.json`, JSON.stringify(tplObj), 'utf-8', err => {
    if (err) console.log(err)
    console.log('\n')
    console.log(chalk.green('Added successfully!\n'))
    console.log(chalk.grey('the lastest template is: \n'))
    console.log(tplObj)
    console.log('\n')
  })

})

```

`tr add [template-name]` 添加一个模板，包括模板的URL地址，让脚手架知道去哪里拉取模板，添加的模板信息可以在template.json里查看到

### remove.js

```javascript
#!/usr/bin/env node
const inquirer = require('inquirer')
const chalk = require('chalk')
const fs = require('fs')
const tplObj = require(`${__dirname}/../template`)


let question = [
  {
    name: "name",
    message: "请输入要删除的模板名称",
    validate (val) {
      if (val === '') {
        return 'Name is required!'
      } else if (!tplObj[val]) {
        return 'Template does not exist!'
      } else  {
        return true
      }
    }
  }
]

inquirer
  .prompt(question).then(answers => {
    let { name } = answers;
    delete tplObj[name]
    // 更新 template.json 文件
    fs.writeFile(`${__dirname}/../template.json`, JSON.stringify(tplObj), 'utf-8', err => {
      if (err) console.log(err)
      console.log('\n')
      console.log(chalk.green('Deleted successfully!\n'))
      console.log(chalk.grey('The latest template list is: \n'))
      console.log(tplObj)
      console.log('\n')
    })
  })
```

`tr remove [template-name]` 删除模版

### list.js

```javascript
#!/usr/bin/env node
const tplObj = require(`${__dirname}/../template`)
console.log(tplObj)
```

最重要的init.js文件

### init.js
```javascript
#!/usr/bin/env node

const program = require('commander')
const chalk = require('chalk')
const ora = require('ora')
const download = require('download-git-repo')
const tplObj = require(`${__dirname}/../template.json`)

program
  .usage('<template name> [project-name]')

program.parse(process.argv)

// 当没有输入参数的时候给个提示
if (program.args.length < 1) return program.help()

// 好比 vue init webpack project-name 的命令一样，第一个参数是 webpack，第二个参数是 project-name
let templateName = program.args[0]
let projectName = program.args[1]

if (!tplObj[templateName]) {
  console.log(chalk.red('\n Template dose not exit! \n'))
  return
}

if (!projectName) {
  console.log(chalk.red('\n Project should not be empty! \n'))
  return
}

const url = tplObj[templateName]

console.log(chalk.white('\n start generating... \n'))

//出现加载图标
const spinner = ora('Downloading...')
spinner.start()

// 执行下载方法并传入参数
download(`direct:${url}`, projectName,{ clone: true}, err => {
  if (err) {
    spinner.fail();
    console.log(chalk.red(`Generation failed. ${err}`))
    return
  }
  // 结束加载图标
  spinner.succeed();
  console.log(chalk.green('\n Generation completed!'))
  console.log('\n To get started')
  console.log(`\n    cd ${projectName} \n`)
})
```

ok, 现在你可以在你的git仓库里新建一个项目模板

然后在我们控制台里 `tr add`，然后输入模版名字，模板的url，tempalte.json里就会有这个模板的信息了。

然后  `tr init [刚刚添加的模板名字] [项目名]`

你就会看见，你的git上的模板被下载了下来。

## 把自己写的脚手架发布到npm上去

* 在根目录下新建 .npmignore 文件，并写入 /node_modules，意思就是发布的时候忽略 node_modules 文件夹

* 去 npm 官网注册个账号（很简单的），同时搜索一下 tr-cli 这个名字，看看有没有人用，有的话就换一个

* 现在让我们回到项目根目录，执行 npm login 登入 npm 账号，再执行 npm publish 发布

发布成功后，就可以在本地 `npm install tr-cli -g` 全局安装一下， 然后执行 `tr` 命令看看