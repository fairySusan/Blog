npm version 8.x
* npm run
  - 每当执行`npm run`命令的时候，就会自动新建一个shell，在这个shell里面执行指定的脚本命令。因此只要是shell可以运行的命令就可以写在npm脚本里。
    比较特别的是`npm run`新建的这个shell会将当前目录的`node_modules/.bin`子目录加入PATH变量，执行结束后，再将PATH变量恢复原样。这就意味着当前目录的
    `node_mudules/.bin`子目录里面所有的脚本，都可以直接用脚本名来调用，而不必加上路径。比如，当前目录的依赖有`cross-env`，只要直接写`cross-env ***`就可以了。
    
* npm bin 打印 npm 将安装可执行文件的文件夹。
* npm bugs [<pkgname> [<pkgname> ...]] 执行这个命令，可以直接浏览器打开指定的包的github的issues页面, 例
  `npm bugs @amap/amap-vue` 等同于 `npm issues @amap/amap-vue`
* npm cache add/clean/clear/verify 关于npm缓存的，一般不要去动它
* npm ci, 这个命令跟npm install命令类似，但是有几处不同点
  - 使用npm ci命令时项目必须要有package-lock.json or npm-shrinkwrap.json 文件
  - 如果package-lock.json文件和package.json文件匹配不上时，npm ci 会抛出错误，而npm install 不会抛出错误，会去更新package-lock.josn
  - npm ci 只能下载整个node_modules文件，不能下载单独的某个包
  - 如果node_modules已经存在了，执行npm ci,会先移除node_modules,再重新下载
  - 在下面情况下npm ci 比npm install要明显快一些
    + 项目中存在package-lock.json or npm-shrinkwrap.json 文件
    + node_modules文件夹不存在或者为空的时候
* npm install 下载包的命名， 何为一个包？
  - 一个包含package.json文件的程序文件夹
  - 一个包含package.json文件的程序文件夹的gzip的压缩文件
  - 一个指向"包含package.json文件的gzip压缩文件"的url
  - 一个<name>@<version>发布在npm仓库上的
  - 一个<npm>@<tag>发布在npm仓库上的
  - 一个<npm>@latest
  - 一个<git remote url>
默认情况下，npm install 下载的是dependencies的依赖
  - npm install -P/--save-prod: 这是默认值 等同与npm install
  - npm install -D/--save-dev: 包会出现在devDependencies里
npm install <别名>@npm:包名，给下载的包取别名，这样就可以下载同名的不同版本的包了
  - npm install my-react@npm:react
  - npm install jquery2@npm:jquery@2
  - npm install jquery3@npm:jquery@3
* npm install sax@latest
* npm install sax@0.1.1
* npm install sax@">=0.1.0<0.2.0" 下载某个范围的版本
* npm install <git remote url> 下载git上的包
  - npm install git+ssh://git@github.com:npm/cli.git#v1.0.2
  - npm install git://github.com/npm/cli.git#v1.0.27
* npm install 下载的是一个可执行文件的话，它会把可执行文件放到`node_modules/.bin/`下,然后使用`npx xxx`来运行它
* npm install -g 下载包到全局环境，这样下载的可执行文件可以在终端里直接运行该包的命令，若是下载在项目里的依赖包，则可用npx来运行依赖包的命令

* npm config 查看npm的配置
  - npm config list
  - npm config set ... 设置另外的npm仓库的命令：npm config set -g cnpm --registry=https://registry.npm.taobao.org
  - npm config get ...
  - npm config delete ...

* npm docs 包名 或者 npm home 包名， 直接浏览器打开指定的包的文档页面 
* npm view vant 查看某个包的信息
  - npm view [package_name] version查看软件包在npm仓库上最新的可用版本
* npm update 更新某个包同时会更新package.lock.json文件,并且npm update不会更新主版本
* npm list 查看所有的已经安装的软件包（包括它们依赖包）的最新版本
  - npm list -g 查看全局安装的软件包

* npm的语义版本控制
  - 语义版本控制的概念很简单，所有的版本都有3个数字：x.y.z
    * 第一个数字是主版本,当进行不兼容的API更改时，则升级主版本
    * 第二个数字是次版本，当以向后兼容的方式添加功能时，则升级次版本
    * 第三个数字是补丁版本， 当进行向后兼容的缺陷修复时则升级补丁版本
  - 运行`npm update [package_name]`的时候，npm会根据package.json里dependencies里软件包的版本号的符号来确定更新方式
    * ^ 只能更新到次版本号和补丁版本号的最新版，不能更新主版本号
    * ~ 只能更新补丁版本
    * '>' 接受高于指定版本的任何版本
    * '>=' 接受等于或高于指定版本的任何版本
    * '<=' 接受等于或低于指定版本的任何版本
    * '<' 接受低于指定版本的任何版本
    * = 接受确切的版本
    * - 接受一定范围的版本 2.1.0-2.6.2
    * || 组合 <2.1 || > 2.6
    * 无符号 1.2.0 只能是这个版本,npm update 不会更新它的版本
    * latest 可用的最新的版本

* npx
  - 使用npx命令可以让你无需install这个依赖包，就可以执行这个依赖包的命令，例如`npx cowsay`
  - 执行npx的时候如果我们没有下载过这个依赖包，那么它会先帮我们下载，当执行完命令后，这个依赖包会被清除
  - npx 还可以直接后面借一个链接来执行代码 `npx  https://gist.github.com/zkat/4bc19503fe9e9309e2bfaa2c58074d32`