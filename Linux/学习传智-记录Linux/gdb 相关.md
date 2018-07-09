
# gdb

## gdb 第一步 加入 -g 参数

	gcc a.c b.c c.c -o app -g

	-g 会保留函数名和变量名

## 启动 gdb

	gdb 可执行程序的名字
		如： gdb app
	
	给程序传参： set args 传递的参数1 传递参数2 ...

		如： set args aaa bbb 111

		输入： r 即可查看程序运行结果


		输入： l 查看源码	默认是查看有main函数的文件
			默认可以查看 10 行
			set listsize 20	可以查看的行数

		l 文件名	可以查看对应的文件
		l 文件名: 行号 查看对应的行号内容
		l 文件名： 函数名		查看对应函数内容

		l 有main函数的文件名 可以回到main函数
		

		输入：q 退出


## 查看代码  --list

	当前文件：	l -- 相当于list
		l
		l 行号
		l 函数名

	非当前文件：
		l 文件名： 行号
		l 文件名: 函数名

	设置显示的行数：
		set listsize 行数


## 断点操作	-break/b

	设置断点：
		b 行号
		b 函数
		b 文件名：行号
		b 文件名：函数名



	查看断点：
		info( i ) b
	
	删除断点：
		d num(断点的编号）
	
		删除多个：
			d num1 num2
			d num1-num5	删除num1 到 num5 断点
		

	设置断点无效：
		dis num（断点编号）

		dis num1 num2 或 dis num1-num3	

	断点生效：
		ena num（断点编号）

	设置条件断点
		b 行号 if 变量 == 值



## 调试相关命令

	让gdb跑越来
		start	停在第一行
		run		停在第一个断点处

	打印变量的值：
		p 变量名

	打印变量的类型：
		ptype 变量名

	向下单步调试：
		next/n	不会进入函数体

		step/s	遇到函数会进入函数体
			finish	退出函数体
				如果退不出函数，可能是函数体中有断点，删除断点或让断点无效，再退出。

	
	继续运行gdb，停止下一个断点的位置：
		continue/c


	变量自动显示：
		display 变量名
		取消显示： undisplay 编号
			i display 可以显示编号

	从循环体中直接跳出：
		until	
			不能有断点，有要取消断点

	直接设置变量等于某一个值：
		set var 变量名 = 某个值














