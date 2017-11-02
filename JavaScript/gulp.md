
# gulp 使用


- 使用 cnpm 替换 npm:
	 
		npm install -g cnpm --registry=http://r.cnpmjs.org
		npm install microtime --registry=http://r.cnpmjs.org --disturl=http://dist.cnpmjs.org

- npm 常用命令

		npm -v          #显示版本，检查npm 是否正确安装。
		 
		npm install express   #安装express模块
		 
		npm install -g express  #全局安装express模块
		 
		npm list         #列出已安装模块
		 
		npm show express     #显示模块详情
		 
		npm update        #升级当前目录下的项目的所有模块
		 
		npm update express    #升级当前目录下的项目的指定模块
		 
		npm update -g express  #升级全局安装的express模块
		 
		npm uninstall express  #删除指定的模块



* 安装
 
    	npm install --global gulp / cnpm install gulp -g

* 作为项目的开发依赖（devDependencies）安装：
* 
    	npm install --save-dev gulp
    
* 在项目根目录下创建一个名为 gulpfile.js 的文件：
* 
	    var gulp = require('gulp');
	
	    gulp.task('default', function() {
	      // 将你的默认的任务代码放在这
	    });


* 运行 gulp：
    	
		gulp

    默认的名为 default 的任务（task）将会被运行，在这里，这个任务并未做任何事情。

    想要单独执行特定的任务（task），请输入 gulp <task> <othertask>。
    如：gulp default


## gulp 的一些插件

* gulp-less 用来将less文件转换css文件
        
		npm install gulp-less --save

* 创建本地服务器 gulp-connect  

      	cnpm gulp-connect -D

* js css 代码压缩相关
   		
		npm install gulp-clean-css gulp-uglify gulp-concat gulp-rename gulp-jshint –save-dev

* webpack一起用
		
		npm install webpack@latest -S
	@latest 表示最新版的


* 安装http-server 一个微型服务器
* 
	    npm install http-server -g
	    使用 : http-server src    // src 表示主要访问的目录
	    可以修改使用的端口号如：http-server src -p 8888

* 安装 browser-sync 可以同时自动刷新多个设备
    
	    npm install -g browser-sync
	    启动：browser-sync start --server "src" --files "src"

* 打包压缩工具
    
		npm install gulp-csso --save-dev
	    
	    gulp-rev gulp-rev-replace gulp-useref gulp-filter gulp-uglify gulp-csso
	
	    gulp-rev 
	    gulp-rev-replace 
	    gulp-useref 
	    gulp-filter 
	    gulp-uglify 
	    gulp-csso






