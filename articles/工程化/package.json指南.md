### package.json的属性解释
  * name 软件包的名称
  * author 列出软件包的作者的名称
  * version 软件包的当前版本，'3.0.0'，第一个数字是主版本号（有重大的修改），第二个数字是次版本号（引入向后兼容的更改的版本），第三个数字是补丁版本号（修复缺陷）
  * main 软件包的入口点，当在应用程序中import这个软件包时，应用程序会砸hi该位置搜索模块的导出
  * private 设置为tru，可以防止应用程序/软件包被意外的发布到npm上
### package-lock.json的作用
  * 在package.json中如果依赖版本写的是这样的：`~0.13.0`，意思是下一次npm install的时候如果这个包有更新新的补丁版本`0.13.1`，则可以下`0.13.1`的版本,0.14.0则不能下载
    如果写入的是`^0.13.0`,则只更新补丁版本和次版本，及`0.13.1`,`0.14.0`都可以更新
    如果写入的是`0.13.0`，则就固定使用这个版本，不能更新版本
    但是一个组的其他人员npm install后，依赖包的版本就可能更新，导致版本不一致带来意想不到的问题，为了解决这个问题，package-lock.json出现
  * 在npm版本5中引入了package-lock.json文件,package-lock.json会固化每个软件包的版本，当运行npm install的时候，npm会使用这些确切的版本。只有
    当npm update的时候package-lock.json文件中的依赖的版本才会被更新