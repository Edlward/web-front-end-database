# react-native 开发相关配置

* 安装react-native-cli：	npm i react-native-cli -g

* 初始化项目 ： react-native init testApp

* 安装android-studio 最新版已经自带SDK， 不用另外安装SDK

* 进入SDK的安装目录 打开 SDK Manager.exe 安装对应的开发配置（这可以百度看看需要安装些什么）

* 安装好配置后，进入项目目录，运行 react-native start 等待一会，在浏览器中打开 http://localhost:8081/index.android.bundle?platform=android 会显示对应的字符串，如果可以访问表示服务器端已经可以了。

* 安装 Android模拟器Genymotion，安装完成之后要启动模拟器，之后用 Android Studio打开对应的项目，并点击上面 有 app显示的绿色向右的三角，等一会就会出现刚才启动的选择器，选择那个选择器点击OK即可。



## 模块

### 1. react-native-tab-navigator

	安装这个模块就可以用 TabNavigator
	安装：npm i react-native-tab-navigator --save
	引用：import TabNavigator from 'react-native-tab-navigator';

### 2.项目用指定react-native版本方法

	react-native init MyApp --version 0.44.3。注意版本号必须精确到两个小数点。

### 3. 项目运行方法

	执行： npm install	安装开发环境
		  react-native start	开启服务器

	之后用Android SDK 打开项目，运行项目即可。
	运行之前要先打开virtual box 上的虚拟设备或者连接到真机上。


## 遇到的问题

### 1.红屏问题"Could not get BatchedBridge, make sure your bundle is packaged correctly"

	解决方法: 
	在package.json中的"scripts"中添加
	"bundle-android":"react-native bundle --platform android --dev false --entry-file index.android.js --bundle-output android/app/src/main/assets/index.android.bundle --sourcemap-output android/app/src/main/assets/index.android.map --assets-dest android/app/src/main/res/"
	如果没有assets目录,手动添加下,然后在cmd中手动执行 上面这个命令，之后Ctrl+c退出, assets目录中会多出几个文件。然后重新启动模拟器，再连接模拟器。

### 2.红屏 连接不到服务器

	在启动模拟器的时候要在命令等运行 react-native start 先启动服务器，这样就不会出现连接不到服务器的问题。在模拟器中按2下R键可以刷新数据。	

### 3.react-native init project 出错

	先执行下面语句，再执行 react-native init project

	npm config set registry https://registry.npm.taobao.org
	npm config set disturl https://npm.taobao.org/dist
 
### 4.Error:java.util.concurrent.ExecutionException: com.android.ide.common.process.ProcessException:

	在 android/app/build.gradle 文件中 android 加入
	aaptOptions.cruncherEnabled = false     
	aaptOptions.useNewCruncher = false
	如下：
	android {  
      
   		......  
  
    	aaptOptions.cruncherEnabled = false  
    	aaptOptions.useNewCruncher = false  
  
   		......  
	}  

### 5. 如果出现 包不能导入的情况

	这可能是包是用cnpm 安装的，需要重新用npm进行安装一次


### 6. 如果 android 虚拟机上不更新代码，换一个虚拟机看看


### 7. reactNative 使用TabNavigator 出现react.children.only expected to receive a single react element child.问题
	
	因为TabNavigator.Item必须全部有组件 需要给每个TabNavigator.Item中返回一个组件

### 8. react-native start / npm start 出现下面问题
	
	 ERROR  EPERM: operation not permitted, lstat 'D:\Workspace\mobile web\react-native\baseDemo4\.idea\workspace.xml___jb_old___'
	{"errno":-4048,"code":"EPERM","syscall":"lstat","path":"D:\\Workspace\\mobile web\\react-native\\baseDemo4\\.idea\\workspace.xml___jb_old___"}
	Error: EPERM: operation not permitted, lstat 'D:\Workspace\mobile web\react-native\baseDemo4\.idea\workspace.xml___jb_old___'
	
	See http://facebook.github.io/react-native/docs/troubleshooting.html
	for common problems and solutions.

解决方案：打开项目中 node_modules/react-native/local-cli/server/server.js 找到 process.on('uncaughtException', error => { 这个方法，把最后一句 process.exit(11); 注释掉。
