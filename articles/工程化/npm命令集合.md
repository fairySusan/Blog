npm version 8.x
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

* npm config 查看npm的配置
  - npm config list
  - npm config set ... 设置另外的npm仓库的命令：npm config set -g cnpm --registry=https://registry.npm.taobao.org
  - npm config get ...
  - npm config delete ...

* npm docs 包名 或者 npm home 包名， 直接浏览器打开指定的包的文档页面 
* npm view vant 查看某个包的信息
* npm update 更新某个包