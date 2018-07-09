
# makefile

## make

	gcc - 编译器
	make - linux自带的构造器
		构造的规则在makefile中



## makefile 文件的命名

	-makefile

	-Makefile	只能是这两种命名方式


## makefile 中的规则

	gcc a.c b.c c.c -o app

	三部分：目标，依赖，命令

	格式：	
	目标：依赖
		（tab缩进）命令

	如：
	app: a.c b.c c.c
		gcc a.c b.c c.c -o app


	makefile中由一条或多条规则组成


## makefile 的编写

	第一个版本：
	app:add.c sub.c hello.c
		gcc add.c sub.c hello.c -o app

	缺点：效率低，修改一个文件，所有文件都会重新编译


	第二版本：

	app:add.o sub.o hello.o
		gcc add.o sub.o hello.o -o app

	add.o:add.c
		gcc add.c -c
	
	sub.o:sub.c
		gcc sub.c -c
	
	hello.o:hello.c
		gcc hello.c -c

	缺点：冗余


	makefile工作原理：
		检测依赖是否存在
			不存在，向下搜索下边的规则，如果有规则是用来生成查找的依赖的，执行规则中的命令。
			
			依赖存在，判断是否需要更新。
				原则：目标时间 > 依赖的时间 不需要更新，反之则需要更新。


	第三版本：

		自定义变量： 变量没有类型，一般小写即可，这样和makefile自带的变量区别。
			obj=a.o b.o c.o
			obj=10
			
		变量的取值： 要加$() 符
			aa=$(obj)

		makefile 自带的变量：这些变量都是大写
			CPPFLAGS
			CC
	
		自动变量：
			$@: 规则中的目标
			$<: 规则中的第一个依赖
			$^: 规则中的所有依赖
			
			自动变量只能在规则的命令中使用
  
		makefile写法：
		obj  = add.o sub.o hello.o
		target = app
		$(target): $(obj)
			gcc $(obj) -o $(target)

		%.o: %.c
			gcc -c $< -o $@


		模式匹配：
		%.o: %c
		第一次：add.o没有
			add.o: add.c
				gcc -c main.c -o main.o

		第二次：sub.o没有
			sub.o: sub.c
				gcc -c sub.c -o sub.o

		...第n次，直到生成所有的目标


		缺点：可移植性比较差

	
	第四个版本：

		用makefile中的函数：makefile中所有的函数都有返回值

		查找指定目录下指定类型的文件 -- wildcard
			如：src = $(wildcard ./*.c )		查找当前目录下所有.c文件，将查找的结果存到src里

		匹配替换 -- patsubst
			如： obj = $(patsubst %.c, %.o, $(src))
				表示要将.c文件替换成.o文件，文件来源来自上次查找中的数据src 

		makefile内容：

		src  = $(wildcard ./*.c)  
		obj = $(patsubst %.c, %.o, $(src) )  
		target = app

		$(target): $(obj)
			gcc $^ -o $@
		
		%.o: %.c
			gcc -c $< -o $@

		缺点：不能清理项目


	第五个版本：

		编写一个规则，清理项目中之前产生的文件

			clean:
				-rm *.o app


		声明伪目标：
			.PHONY: clean	--声明clean为伪目标

		-f: 强制执行

		命令前面加 - 
			忽略执行失败的命令，继续执行下面的命令
			如： 
				-rm *.o app
				echo "hello,world"

				表示如果，删除命令失败，继续执行下面的输出操作


		让make生成不是终极目标的目标
			make 目标名
			如删除文件操作： make clean	只是执行地makefile中的clean，不生成终极目标

		makefile内容：

		src  = $(wildcard ./*.c)  
		obj = $(patsubst %.c, %.o, $(src) )  
		target = app
		
		$(target): $(obj)
			gcc $^ -o $@
		
		%.o: %.c
			gcc -c $< -o $@
		
		
		hello:
			echo "hello,world"
		
		.PHONY: clean
		clean:
			-rm $(obj) $(target) -f


          
## makefile 文件执行

	在shell中输入： make	即可。



## makefile 中注释 --用 # 

	如：
	# xxx	表示这是注释






















