
### angular-cli 安装 在win10系统 

	1、查看你的node以及npm版本：
      node -v    查看node版本
      npm -v      查看npm版本

**npm node 要求所有版本都是最新的，不然可能会出错。**

	2、windows 系统要安装vs2015或者其他的vs系列，并且要安装python2.7,
	python2.7要安装在系统默认路径下，C:\Python27
	并且可能要自己下载设置 win32-x64-46_binding.node 文件路径
	如下：set SASS_BINARY_PATH=D:\Complete\node-sass-binaries\win32-x64-46_binding.node
	我是将这个文件下载放在D盘对应目录下。然后再安装 node-sass
	查看环境是否合适：echo %SASS_BINARY_PATH%
	
	如果打印出来您配置好的文件地址那就ok了，
	
	最后再来试试安装：npm i -g node-sass

	
	
	3、因为angular-cli是用typescript写的，所以要先装这两个：
	   npm install -g typescript typings
	
	 4、安装angular-cli:
		
		npm install -g @angular/cli

	   如果你之前安装失败过，最好在安装angular-cli之前先卸载干净，用以下两句：
      npm uninstall -g @angular/cli
      npm cache clean --force
	  或者 npm cache verify
	
	同时，在检查你全局的那些npm文件下还残留下图这两个文件，
	在这目录下载 
	
	C:\Users\hxh\AppData\Roaming\npm\node_modules

	有的话也要删掉，删掉后再用 npm install -g angular-cli安装最新的
	angular-cli即可。
	
	卸载：
	npm uninstall -g @angular/cli

## 在ubuntu 安装 @angular/cli
	sudo npm install  @angular/cli -g --unsafe-perm 

## 用 cnpm 安装 @angular/cli 可能就不会现出上面这些问题
	
	cnpm install -g @angular/cli


## 问题
	
	“C:\Users\shawn\.node-gyp\0.10.13\Release\node.lib : fatal error LNK1106: 文件无 
	效或磁 
	盘已满: 无法查找到 0x164FE [D:\node\node_modules\jsdom\node_modules\contextify\b 
	uild\co 
	ntextify.vcxproj] 
	gyp ERR! build error” 
	
	回头把目录 C:\Users\shawn\.node-gyp 删掉,
	然后再卸载 npm uninstall -g @angular/cli，清除缓存
	npm cache clean --force
	然后再安装即可 npm install -g @angular/cli。


### angular-cli 相关命令

- 初始化一个项目： 
	- ng new my-project
- 启动项目，进入对应的项目目录下载，如：
	- cd my-project
	- npm install
	- ng serve --open

	或者：ng serve --prod --aot --open 
	表示进行预编译，代码会小很多


### 编译 和 整合

	ng build
	会生成好一个dist目录，该目录下的文件放到服务器即可。
	不过通常放到生产中要用： ng build --env=prod 命令

### 改变环境：生产或者开发或者测试环境 

	在package.json中修改
    "scripts": {
        "ng": "ng",
        "start": "ng serve --env=prod --proxy-config proxy.conf.json",
        "build": "ng build",
        "test": "ng test",
        "lint": "ng lint",
        "e2e": "ng e2e"
    }
	中的        
	 "start": "ng serve --env=prod --proxy-config proxy.conf.json",
	即可， --env=prod 中的 prod 在 angular-cli.json 中的
	 "environments": {
        "dev": "environments/environment.ts",
        "prod": "environments/environment.prod.ts"
      }
	定义，和这里的相对应即可，然后再在 environments 目录下配置对应的文件。 
	
	在要用到环境相关的配置引用即可：
	import {environment} from "../environments/environment";







### angular 目录结构

- e2e
 	- 用来作测试用
- src --  放源代码文件
	- app -- 项目代码入口
	- assets  --放静态资源
	- environments -- 配置环境变量相关
	- index.html  -- 根HTML
	- main.ts -- 所有脚本的入口文件，index.html加载这个文件即可
	- polyfills.ts -- 导入一些库，配置兼容性相关如IE8，9
	- styles.css -- 放全局的CSS样式
	- test.ts -- 自动化测试
	- tsconfig.app.json --  TypeScript编译器的配置,添加第三方依赖的时候会修改这个文件
	- tsconfig.spec.json -- 不用管
	- typings.d.ts -- 不用管
- .angular-cli.json
	- angular 命令行工具相关
- editorconfig
	- 配置编辑器相关
- gitignore
	- git相关配置
- karma.conf.js
	- 执行自动化测试
- package.json
	- npm 相关的配置，是项目安装包相关
- protractor.conf.js
	- 也是用作自动化测试
- tsconfig.json
	- 作用typescript的代码检测相关
- tslint.json
	- 作用代码规范检测相关

一个Angular程序至少需要一个模块和一个组件。在我们新建项目的时候命令行已经默认生成出来了

### 组件相关的概念：

- 1.组件元数据装饰器（@Component）
简称组件装饰器，用来告知Angular框架如何处理一个TypeScript类.
Component装饰器包含多个属性，这些属性的值叫做元数据，Angular会根据这些元数据的值来渲染组件并执行组件的逻辑

- 2.模板（Template）
我们可以通过组件自带的模板来定义组件的外观，模板以html的形式存在，告诉Angular如何来渲染组件，一般来说，模板看起来很像html，但是我们可以在模板中使用Angular的数据绑定语法，来呈现控制器中的数据。

- 3.控制器（controller）
控制器就是一个普通的typescript类，他会被@Component来装饰，控制器会包含组件所有的属性和方法，绝大多数的业务逻辑都是写在控制器里的。控制器通过数据绑定与模板来通讯，模板展现控制器的数据，控制器处理模板上发生的事件。

- 4.装饰器，模板和控制器是组件的必备要素。还有一些可选的元素，比如：

	- 输入属性（@inputs）:是用来接收外部传入的数据的,Angular的程序结构就是一个组件树，输入属性允许在组件树种传递数据
	- 提供器（providers）：这个是用来做依赖注入的
	- 生命周期钩子（LifeCycle Hooks）：一个组件从创建到销毁的过程中会有多个钩子会被触发，类似于Android中的Activity的生命周期
	- 样式表：组件可以关联一些样式表
	- 动画（Animations）： Angular提供了一个动画包来帮助我们方便的创建一些跟组件相关的动画效果，比如淡入淡出等
	- 输出属性（@Outputs）：用来定义一些其他组件可能需要的事件或者用来在组件之间共享数据


### angular4 引用第三方的库

	1. npm install jquery --save
	2.  在 angular-cli.json 中的 scripts 或者styles中引入
		      "styles": [
			"styles.css",
			// "../node_modules/bootstrap/dist/css/bootstrap.css"
			"bootstrap.css"
		      ],
		      "scripts": [
			"jquery.js",
			"bootstrap.js"
			// "../node_modules/jquery/dist/jquery.js",
			// "../node_modules/bootstrap/dist/js/bootstrap.js"
		      ],

	3. 安装对应的类型描述文件
		npm install @types/jquery --save-dev
		npm install @types/bootstrap --save-dev


### angular 生成组件方法
	ng g c navbar  -- 生成navbar组件，并会自动引入组件


### *ngFor 和 类的使用
	<div>
		<span *ngFor="let star of stars" class="glyphicon glyphicon-star"
		    [class.glyphicon-star-empty]="star"
		>
		</span>
		<span>{{rating}} 星</span>
	</div>
	stars 为数组，值为boolen，star 为boolen值，当这个值为真的时候，加入 glyphicon-star-empty 这个类

### angular 中属性的绑定是通过用 [] 来的

	<app-starts [rating]="product.rating"></app-starts>
	app-starts zhujian组件需要 rating数据，父组件通过 属性绑定传递数据过去，父组件也可以直接传递数据过去，如： <app-stars rating="3"></app-stars>
	也就是 通过属性绑定的是变量，值的话可以直接传递。


### angular router 相关

	创建项目：ng new demo --routing
	
	路由中的path 路径部分不能加入 / ，要如下面所示，直接写路径名称
	const routes: Routes = [
		{
			path: '',
			component: HomeComponent
		},
		{
			path: 'product',
			component: ProductComponent
		}
	];
	path 中什么也不写，表示的是根路由
	
	在html中引用路由的方法：
	
	<a [routerLink]="['/']">主页</a>
	<a [routerLink]="['/product']" [routerLinkActive]="['active']">商品详情</a>

	<!-- 路由内容显示区域 -->
	<router-outlet></router-outlet>
	
	/ 就是表示 访问根路由 ~~~~

	[routerLinkActive]="['active']" 表示在点击跳转到路由时加载active类 active类 是自己定义的
	
	// 导航到 home 页面
            this.router.navigate(['/home']);

### 路由导航
	
	这是绝对导航，是相对根目录的
	this.router.navigateByUrl("posts/page/"+temp);

	这是相对导航：

	this.router.navigate(['/child-center'], { relativeTo: true})

	这里就是相对于当前route(this.route/true)进行导航，导航至/child-center。
	如果没有后面的部分就是相对根目录跳转 ： this.router.navigate(['child-center'])
	
	如果当前url是xxx:8080，那么导航后的结果： 
	xxx:8080/child-center
	
	也可以导航至上一层
	
	    this.router.navigate(['../'], { id: 1, foo: 'foo' }, {relativeTo: this.route})


### implements
	

## 依赖注入--提供器

- 如果提供器写在 app.module.ts中，则所有 的组件都可以使用

			providers: [ProductService], 这个是放在app.module.ts中 @NgModule 里
			
			在组件中这样调用 即可：
			import { Component, OnInit } from '@angular/core';
			import {Product, ProductService} from "../shared/product.service";

			@Component({
			  selector: 'app-product1',
			  templateUrl: './product1.component.html',
			  styleUrls: ['./product1.component.css']
			})
			export class Product1Component implements OnInit {

			    product: Product;

			    constructor(private productServe: ProductService) { }

			    ngOnInit() {
				this.product = this.productServe.getProduct();
			    }
			}	
	
- 写在组件中的，话只有组件自己可以使用

		import { Component, OnInit } from '@angular/core';
		import {Product, ProductService} from "../shared/product.service";
		import {AnotherProductService} from "../shared/another-product.service";

		@Component({
		    selector: 'app-product2',
		    templateUrl: './product2.component.html',
		    styleUrls: ['./product2.component.css'],
		    // 主要多了这个，部分，初始化时用 AnotherProductService而不是原来的ProductService
		    providers: [{
			provide: ProductService,
			useClass: AnotherProductService
		    }]
		})
		export class Product2Component implements OnInit {

		    product: Product;

		    constructor(private productService: ProductService) { }

		    ngOnInit() {
			this.product = this.productService.getProduct();
		    }
		}

- 依赖注入组件调用时，只能通过构造函数来引用

		export class Product2Component implements OnInit {

		product: Product;
		// 只能在这里声明一个 依赖注入的 组件
		constructor(private productService: ProductService) { }

		ngOnInit() {
		this.product = this.productService.getProduct();
		}

		}

### 数据绑定

	angular 数据绑定默认是单向的 ，也可以设置成双向

### ngModel 指令 

	要用这个指令需要在 app.module.ts 中加入 
	 import { FormsModule } from '@angular/forms';
	 并且在 @NgModule 中加入 FormsModule
	 
	@NgModule({
	    declarations: [
		AppComponent,
		BindComponent
	    ],
	    imports: [
		BrowserModule,
		FormsModule
	    ],
	    providers: [],
	    bootstrap: [AppComponent]
	})
	
	而且ngModule只能在 input 等表单中使用

### 创建 自定义 管道

	ng g p pipe/multiple
	创建一个muliple 管道在pipe目录下
	系统会自动 在 app.molule.ts  NgModule内加入刚创建的管道 
	
	@NgModule({
	    declarations: [
		AppComponent,
		BindComponent,
		MultiplePipe  // ---刚创建的管道
	    ],
	    imports: [
		BrowserModule,
		FormsModule
	    ],
	    providers: [],
	    bootstrap: [AppComponent]
	})
	用的时候 用 multiple 这个名字即可：
	<p> {{ testVal | multiple: '2'}} </p>  后面的 2的参数，也可以没有 参数
	<p> {{ testVal | multiple}} </p>
	
	multiple.pipe.ts文件内容 如下：
	
	import { Pipe, PipeTransform } from '@angular/core';

	@Pipe({
	  name: 'multiple'
	})
	export class MultiplePipe implements PipeTransform {

	    transform(value: number, args?: number): any {

		if ( !args ) {
		    args = 1;
		}
		return value * args;
	    }
	}

	
### 表单

	angular表单分为--响应式表单和模板式表单

	响应式表单用作复杂的表单，有校验相关
	模板式表单用作一些简单的操作

	ngForm 指令会阻止表单提交
	在ngForm 里 ngModel 是不用括号的
	ngForm--是模板式表单相关的指令

	响应式表单相关的指令都以form开头--formGroup、formControl

	formGroup 内部只能用 有 name结尾的指令如：formControlName、formArrayName
	同样的 formControlName这些指令也只能用在formGroup之内

- 状态字段

	- touched 和 untouched
	
		当字段没有获取过焦点时touched为false,untouched为true
		获取过焦点时相反，touched为true,untouched为false
	
	- pristine 和 dirty
	
		如果字段的值没有被改变过 pristine=true , dirty=false
		修改过：pristine=false , dirty=true

	- pending
	
		当字段正在校验时，这个值为true，其他时候为false.


### product?.price
	
	这种写法表示：只的当product有值的时候才会云访问，product.price这个值

### import 'rxjs/add/observable/of';



	
	
	
	
	
	
	



















































