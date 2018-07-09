
## 安装 qtcreateor

	http://qt-project.org/	网站下载 或者
	http://download.qt.io/archive/qt/	这个网址有所有版本可以下载


## Ubuntu下卸载QT 再重装 方法

	1. 卸载QT5.7.1 
	
		1.首先找到QT安装文件的位置，例如我的在/home/ttwang/software/qt5.7.1
		
		2.终端输入命令进入该目录，输入命令：
		    ./MaintenanceTool      进入图形画面卸载就行了


	2. 安装QT5.7.1 

		1.下载 QT5.7.1   下载链接
		
		2. 将下载的安装文件qt-opensource-linux-x64-5.7.1.run拷贝到home/用户目录，如/home/ttwang, 这里我指定安装目录，，拷贝到/home/ttwang/software/qt5.7.1
		
		3.进入该目录，添加执行条权限：
		   chmod u+x qt-opensource-linux-x64-5.7.1.run
		
		4.在终端执行：
		 ./qt-opensource-linux-x64-5.7.1.run
		
		5.跳出图形安装界面，一步一步安装就行了


        QT启动命令： 
            /home/topeet/Qt5.8.0/Tools/QtCreator/bin/qtcreator
        
        QT出现：undefined symbol: g_type_ensure 错误
            将 platformthemes 改名为其他即可，如 platformthemes_my






