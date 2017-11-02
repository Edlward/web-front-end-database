# NodeJS 学习记录


* 运行 index.js文件：
	先进入文件当前目录，在 cmd 命令行下输入 node index.js

	js 能用的功能 node.js都能用

* 安装nodemon
	npm i -g nodemon
	用这个模块启动服务器，会自动检测服务器文件变化，然后重启服务器
	nodemon index.js


* 启动一个服务器 

		const http = require('http');
		// import http from 'http'
		
		var server = http.createServer(function (request, response) {
		    console.log('server start...');
		});
		
		// 监听---端口
		server.listen(8089);

		request--请求 输入-请求的信息
		response--响应 输出的数据

* 文件操作： fs--file system

## 问题：events.js:182       throw er; // Unhandled 'error' event...

	多半是端口被占用，如 8080端口被占用，解决如下：
	在cmd 中输入：
	netstat -ano |findstr "8080"
	查看最右边的PID 用 tasklist | findstr PID 命令查看端口被哪个程序占用
	如：tasklist | findstr 11464 这是之前占用8080端口的程序，然后在
	任务管理器中关掉对应的程序即可。



### cookie

	在浏览器保存一些数据（在客户端），每次请求都会一起传到服务器 
	不安全、有限（4K）

	1.读取cookie --cookie-parser
		server.use(cookieParser(密钥)); 如：
		server.use(cookieParser('adfke1113w3'));

	2.发送cookie

 		req.secret=加密密钥; 如：req.secret="adfke1113w3";
		res.cookie('user', 'value', {
        	// 签名，显示加密的cookie，但是会耗空间
        	signed: true
    	});

	3.删除cookie

	res.clearCookie('user'); user为cookie 名
	
### session
	
	保存数据，保存在服务器端，大小没有限制
	基于cookie实现 
	cookie中会有一个session的ID，服务器利用session ID 找到session文件，读取、写入
	隐患：session 劫持

### 模板引擎 智能12

- jade --破坏式、侵入式、不能用HTML
- ejs -- 非侵入式、温和

	不管是jade还是ejs，代码内部的 空格 都会原样输出

### 1.jade

	1.根据缩进，规定层级
	
	2.相关写法

	style比较特殊 可以用 普通写法和 json写法
	    div(style="width: 200px; height: 200px")   
        div(style= {width: '200px', height: '200px', background: 'red'}) 

	class 可以肜普通写法和 数组写法

		div(class="aaa")
        div(class=["bbb", "ccc"])

	简写：
	    div.box1
        div#box2
        div(title="this is title", id="div11")
        div#div22(title="this is title")
	
	可以自动识别单双标签

	原样输出 1.在前面加 |
		html
    		head
    			script(src='a.js')
		    	script 
			        |window.onload=function(){
			        |   let btn = window.getElementById("btn");
			        |};
    			link(href='a.css', rel="stylesheet")
    		body

		        |this is data 
		        |aaaaaaa
		        | bbbbbb

		2.在后面加 . 也可以
		
		html
    		head
    			script(src='a.js')
		    	script. 
			        window.onload=function(){
			           let btn = window.getElementById("btn");
			        };
    			link(href='a.css', rel="stylesheet")
    		body

		        |this is data 
		        |aaaaaaa
		        | bbbbbb

			. 表示后面的 下一级 下所有的东西都原样输出


- -表示后面是一段代码，不进行转义，直接处理

	    -var a=10

        -var b=5    

        div 我的名字是：#{name}

        div #{a+b}

	如果后面都是代码，只要第一行中有一个 - 就行，下面的switch-cast 

        -var a= 2;
        case a
            when 0
                div aaa
            when 1
                div bbb
            default
                div 进入default
	

- = 为转义输出 在标签后面相当于将 后面的变量加到标签里
     	
		span=a
        span=b

		和下面这两个 是相同的意思

        span #{a}
        span #{b}		


### consolidate--给express适配模板引擎 
	
		使用方法：

		// 配置模板引擎
		1. 输出什么东西
		server.set('view engine', 'html');

		2. 模板文件放在哪 views是自己配置的目录
		
		server.set('views', 模板文件目录); 如：
		server.set('views', './views');

		3. 哪种模板引擎，这是用ejs
		server.engine('html', consolidate.ejs);
		
		server.get('/', function(req, res) {
			res.render(模板文件，数据)
		}
		如：
		// 用户请求
		server.get('/index', function (req, res) {
		    
		    // 相当于编译过再发送给前端
		    res.render('1.ejs', {name: 'blue'});
		    
		    // 相当于直接发送数据给前端
		    // res.send("xxx");
		
		});


### route--路由
	
	把不同的目录，对应到不同的模块




## node 的更新：

	重新下载新版本的 msi 安装包，然后覆盖安装之前的版本来完成更新操作的。
	
	我们在覆盖的时候要检查之前安装 node 的路径，使用命令  where node 

## ubuntu 安装node.js

	更新ubuntu软件源
	
	sudo apt-get update
	sudo apt-get install -y python-software-properties software-properties-common
	sudo add-apt-repository ppa:chris-lea/node.js
	sudo apt-get update
	
	安装nodejs
	
	sudo apt-get install nodejs
	sudo apt install nodejs-legacy
	sudo apt install npm
	
	更新npm的包镜像源，方便快速下载
	
	sudo npm config set registry https://registry.npm.taobao.org
	sudo npm config list
	
	全局安装n管理器(用于管理nodejs版本)
	
	sudo npm install n -g
	
	安装最新的nodejs（stable版本）
	
	sudo n stable
	sudo node -v

