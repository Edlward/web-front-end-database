
## 安装GCC对应版本和设置版本为默认

	1. 安装6.2版本的gcc/g++（安装时间比较长，请耐心等待）
		sudo add-apt-repository ppa:ubuntu-toolchain-r/test
		sudo apt-get update
		sudo apt-get install gcc-6 g++-6

	2. 设置默认gcc/g++为6.2版本

		g++设置:
		# 命令最后的 200是优先级，如果使用auto选择模式，系统将默认使用优先级高的
		//sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-4.6 20
		sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-6 200
		
		# 使用交互方式的命令选择默认使用的版本
		sudo update-alternatives --config g++
		
		# 查询系统中安装有哪些版本
		sudo update-alternatives --query g++
		
		gcc同理:
		
		sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-6 200
		
		sudo update-alternatives --query gcc
		
		sudo update-alternatives --config gcc


## GCC

	gcc工作流程

		1）预处理	--E

			宏替换
			头文件展开
			注释去掉
			xxx.c --> xxx.i	还是C文件

		2）编译		--S
			
			xxx.i --> xxx.s
					汇编文件
			
		3) 汇编		--c

			xxx.s --> xxx.o	二进制文件


		4）链接	--o 指定生成的文件名

			xxx.o --> xxx（可执行）
				./xxx 	即可运行可执行文件

		可以直接生成可执行文件：gcc sum.c -o sum


	常用命令：

		-v/ --version:	查看gcc版本

		-I:	编译的时候指定头文件路径

			gcc sum.c -I ./include -o sum
			sum.c中有一个头文件在当前include目录下，-o 表示生成可执行文件名称为sum

		-c: 将汇编文件生成二进制文件

		-o:  指定生成 文件的名字
			gcc sum.c -o sum	生成名为sum的可执行文件
				不加-o默认是生成a.out的可执行文件
		
		-g: gdb调试的时候需要加

		-D: 调试用，编译时指定宏，这样程序里可以不定义宏，然后在编译时加入宏，就可以显示调试信息

			gcc sum.c -D DEBUG -o app

		-Wall: 输出警告信息

		-On: 优化代码 n是优化级别：1 2 3	3优化级别最高
			gcc sum.c -o app -O2	这个是大O




## 静态库和动态库的制作
	
	1. 库是什么？
		1. 二进制文件
		2. 将源代码--> 二进制格式的源代码
		3. 加密
	
	2. 库制作出来之后如何使用
		1. 头文件
		2. 制作出的库	有这两个才可以使用
		3. 
	
	3. 静态库的制作和使用
		1. 命名规则	如：libtest.a
			lib	开头
			xxx -库的名字	只有这个是可以自己命名的
			.a	结尾
		2. 制作步骤
			1. 源代码.c .cpp
			2. 将.c生成.o文件	gcc a.c b.c -c 或者： gcc *.c -c
			3. 将.o打包
					1. ar rcs 静态库的名字 需要打包的.o文件	
						如：ar rcs libtest.a a.o b.o  
							或 ar rcs libtest.a *.o	
		3. 库的使用
			1. gcc test.c -I ./include -L ./lib -l mycal -o app
				用静态库将 test.c编译生成 app 可执行文件
				-I 指定test.c中头文件的路径
				-L 指定库的路径，就是这个静态库放在./lib目录下
				-l 小写l, 指定静态的名字 名字去掉lib 和 .a 如：libmycal.a--> mycal即可。


	
	4. 动态库的制作和使用
		1. 命名规则
			1. libxxx.so
		2. 制作步骤
			1. 将源代码生成.o  如：gcc a.c b.c -c -fpic
			2. 打包	如： gcc -shared -o libxx.so a.o b.o
		3. 库的使用
			1. 头文件xx.h	就是动态的头文件
			2. 动态库 libxx.so	如：libtest.so
			3. 参考函数声明编程测试即可，如：测试文件main.c
			4. gcc main.c -I ./include -L ./lib -l test -o app
			
	
	5. 动态库无法加载解决方法：
			1. 使用环境变量
					1. 临时设置
						1. export LD_LIBRARY_PATH=动态库的路径
						2. 或者：export LD_LIBRARY_PATH=动态库路径:$LD_LIBRARY_PATH
					
					2. 永远设置
						1. 用户级别：
							1. ~/.bashrc 将库文件的绝对路径加到这个文件最后即可
								如： export LD_LIBRARY_PATH=动态库的绝对路径:$LD_LIBRARY_PATH
								然后重启终端，因为终端打开才会读取这个文件
								或者 source ~/.bashrc source可以缩写为 . ，所以可以写作： . ~/.bashrc
			
						2. 系统级别：
							1. /etc/profile	加入到这个文件即可 要用sudo  才能修改这个文件
								1. 修改后重启终端或者source /etc/profile
	
				
			2. 更新/etc/ld.so.cache 文件列表
					1. 找到一个配置文件	/etc/ld.so.conf 将动态库的绝对路径写到里面
							只写绝对路径即可： 如： /home/hxh/lib
					2. 执行一个命令: sudo ldconfig -v -- -v可以不加，-v只是显示信息
	
	
	
			3. 将动态库放到 /lib 或者/usr/lib 下即可，但是不怎么用这个方法，因为这可能会覆盖原来的动态库文件
	

	dlopen/dlclose/dlsym用这些也行

	ldd app	可以查看可执行程序app需要加载的库

	在windows下 .lib为静态库，.dll为动态库
	linux下，.a为静态库，.so.1.7为动态库 1.7为版本


