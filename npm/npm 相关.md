
## npm 设置 淘宝 镜像

	npm config set registry https://registry.npm.taobao.org
	npm config set disturl https://npm.taobao.org/dist

## 清除已设置的npm淘宝镜像
	
	npm config delete registry
	npm config delete disturl

## 清除缓存

	npm cache clean --force
	或者 npm cache verify

## npm -g 安装模块所有的目录

	C:\Users\hxh\AppData\Roaming\npm\node_modules

## devDependencies和dependencies的区别

	我们在使用npm install 安装模块或插件的时候，有两种命令把他们写入到 package.json 文件里面去，比如：
	
	--save-dev
	
	--save
	
	在 package.json 文件里面提现出来的区别就是，使用 --save-dev 安装的 插件，被写入到 devDependencies 对象里面去，而使用 --save 安装的插件，责被写入到 dependencies 对象里面去。
	
	那 package.json 文件里面的 devDependencies  和 dependencies 对象有什么区别呢？
	
	devDependencies  里面的插件只用于开发环境，不用于生产环境，而 dependencies  是需要发布到生产环境的。


## 查看项目中模块的版本

- npm outdated--查看所有模块的版本
- npm view jquery versions--查看某个模块的所有版本
- npm v jquery versions --json
	- 想要引入的是Jquery的1.7.2版本，则输入npm install jquery@1.7.2 --save

### 查看所有全局安装的模块

	npm list --depth=0 -global