
## webpack用法：

	1. 进入对应目录 npm init
	2. 	全局安装webpack: npm i webpack -g
		安装webpack到目录下	cnpm install webpack --save-dev
	
	3. 打包文件，如：webpack input.js output.js
		input.js是要加载的js文件，output.js 是输出的文件
	

	4. 如果要打包CSS文件
	
		在项目目录里安装 cnpm install css-loader style-loader --save-dev

		引用方式： require('style!css!./style.css');
		style!css!位置不能写反，是从右边开始处理的。


	5. 如果要在 控制台中 查看到源代码需要用到下面命令
	6. 
		webpack --devtool source-map
		就会在输出目录下 生成对应的.map文件
		
	6.如果用ES6/ES2015的写法，需要安装 babel

		cnpm install babel-loader babel-core babel-preset-es2015 --save-dev
		同时要在项目根目录下 创建 .babelrc 文件，其内容为
		{ "presets": ["es2015]"}
		同时要修改webpack.config.js文件 在 module 里的 loaders 里增加下面这句
		{test: /\.js$/, loader: 'babel-loader'}
		这样就可以使用ES6的语法了。

	7.webpack-dev-server

		可以用来调试，会生成一个服务器，自动打包变化的文件，不用刷新页面
		而且页面只改变修改部分，其他的都不变。
		cnpm install webpack-dev-server --global
		在当前目录也安装
		cnpm install webpack-dev-server --save-dev 
		然后运行，输入下面命令： webpack-dev-server --inline

		webpack-dev-server --port 3000	更改端口号

		--content-base ./bound/ 改写 访问的目录 默认是当前目录(index.html)

	8.自动生成html 文件

		cnpm install html-webpack-plugin --save-dev
		使用时在webpack.config.js里加入：
		var htmlWebpackPlugin = require('html-webpack-plugin');
		// webpack1 写法
		plugins: [
			new htmlWebpackPlugin({
					title: "my app",
					// 生成文件名，默认为index.html
					filename: "index.html",	
					// index.html中加载/使用的js文件
					chunks: [bound.js]
				}),
			new htmlWebpackPlugin({
					title: "my app",
					// 生成文件名，默认为index.html
					filename: "abc.html",	
					// index.html中加载/使用的js文件
					chunks: [abc.js]
				}),
			]
			// webpack2 写法
			new htmlWebpackPlugin({
                    // html源文件 + bundle.js文件生成 bundle.html文件
                    // 输出的文件名称 默认index.html 可以带有子目录
                    // filename: './item/bundle.html',
                    filename: 'bundle.html',
                    // 源文件
                    template: 'index.html',
                    // 注入资源在 head部分，不写就是在body部分加入js文件 
                    //inject:'head',
                    title:'hello webpack',
                    chunks:['bundle'],//指定chunks 为 bundle 的js
                }),
                new htmlWebpackPlugin({
                    // xxx.html源文件 + xxx.js文件生成 xxx.html文件
                    // 输出的文件名称 默认index.html 可以带有子目录
                    // filename: './item/bundle.html',
                    filename: 'abc.html',
                    // 源文件
                    template: 'index.html',
					// 注入资源在 head部分，不写就是在body部分加入js文件 
                    // inject:'head',
                    title:'hello abc',
                    chunks:['abc'],//指定chunks 为 abc.js
                }),



	9.使用其他名称的配置文件，需要在package.json里的

		webpack默认是使用webpack.config.js文件进行配置加载的，如果要使用其他的文件，就要在package.json里处理：
		在 "script" 配置里加入自己的定义，如：
		"start": "webpack --config webpack.my.config.js"
		启动要用: npm run start
	
	10.显示打包进度

	当项目逐渐变大，webpack 的编译时间会变长，可以通过参数让编译的输出内容带有进度和颜色。
	命令： webpack --progress --colors



## webpack.config.js	

	为webpack的配置文件，配置好之后只要输入 webpack即可，其他都在配置文件中说明。





## webpack2  与 webpack 的区别
	
	==================================================================


	webpack..config.js要求必须加-loader后缀，如
	 {test: /\.css$/, loader: 'style-loader!css-loader'}

	注意：不写hot: true，否则浏览器无法自动更新；也不要写colors:true，progress:true等，
	webpack2.x已不支持这些
	
	webpack-dev-server 启动也不用加 --hot 因为 webpack2.xx 不支持这些
	webpack-dev-server --inline



	==================================================================






























