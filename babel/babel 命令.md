# babel 相关命令

安装babel:

	// 5.x
	npm install babel -g 这是全局安装
	npm install babel -D 局部安装
	上面选择一种安装方法即可。

	// 6.x 可以安装 一部分
	cnpm install babel-cli -g
	cnpm install babel-cli -D
	

编译单个文件:

	进入对应目录，babel index.js -o output.js

编译整个目录：

	babel src --out-dir dist

如果要对源文件进行监听可以加 -w ：

	babel src --out-dir dist -w

ES6解释器：

- babel-preset-es2015
- babel-preset-es2015-loose
- babel-preset-es2016

同时要在 .babelrc 文件中进行配置

	{
		"presets": ["es2015", "es2016"]
	}

### babel plugin

主要解决 export default xx;

-babel-plugin-add-module-exports


同时要在 .babelrc 文件中进行配置

	{
		"plugins": ["add-module-exports"]
	}


### babel解析 react jsx 语法 插件

- babel-preset-react


同时要在 .babelrc 文件中进行配置

	{
		"presets": ["es2015", "es2016"， "react"]
	}


### 在 gulp 中使用 babel

- cnpm install gulp-babel gulp -D

 








