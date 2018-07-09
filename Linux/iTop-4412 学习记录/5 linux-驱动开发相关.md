
## 新的linux内核 要编译过，才可以用

	编译命令：
		make oldconfig&&make prepare&&make scripts

	出现选项，一路确认即可。



## 加载模块、查看模块、卸载模块

	– insmod 加载模块命令

	– lsmod 查看模块命令

	– rmmod 卸载模块命令


	如：insmod hello.ko


## android 中不能执行程序，解决方法：

	重新加载该分区，命令： mount -o rw,remount /mnt/udisk1

	然后一切正常，自己的执行程序现在工作正常了。



## 写在android 内核中的主程序不能有返回值

	如：
	int main()
	{
		
		return 0;
	}

	在android中执行的时候出现： Segmentation fault 错误。

	要将返回值去掉：

	main()
	{
		
	
	}

	这样就不会现出错误。







