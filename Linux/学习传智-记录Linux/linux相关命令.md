
# 命令

## 快捷键	在中端的快捷键

	ctrl+h	删除光标前面的一个字符

	ctrl+d	删除光标当前所在的字符

	ctrl+u	删除光标前面的所有字符 

	ctrl+k	删除光标后面的所有字符

	ctrl+p 可以调用之前输入的命令 是从最后输入的命令往上开始显示

	ctrl+n 从上往下 显示之前输入的命令

	移动：
	ctrl+a	移动到头部
	ctrl+e	移动到尾部
	ctrl+b	向前移动一个字符/是向左移动一个字符
	ctrl+f	向后移动一个字符/或者说是向右移动一个字符

## 安装软件方法

	sudo apt-get install 软件名：sudo apt-get install tree

	或者： sudo apt install 软件名  在 ubuntu16.04后应该这样写。

	如果终端提示:
	无法获得锁 /var/lib/dpkg/lock - open (11: 资源暂时不可用) 
	E: 无法锁定管理目录(/var/lib/dpkg/)，是否有其他进程正占用它？”
	
	解决办法如下：
	命令:
	sudo rm /var/cache/apt/archives/lock
	sudo rm /var/lib/dpkg/lock

## ubuntu 打开图形界面终端

	打开： ctrl + alt + t
	退出： ctrl + d

## ubuntu 打开DOS界面终端

	打开：	ctrl + alt + F1 (F1--F6都可以) 
	退出：	ctrl + alt + F7


## su 	--用户间切换

	su - 需要切换到的用户
	如：su - test	切换到test用户

	直接： su 	切换到root用户，输出密码即可。



## 修改root密码

	sudo passwd root

	如果要输入之前的root密码，就输入之前的root密码
	不然，输入再次新密码即可。


## time 	查看程序对应时间

	time ./a.out	
	显示大概如下：
	real	0m1.007s	程序运行的总时间
	user	0m0.008s	用户占用的时间
	sys		0m0.132s	内核占用的时间

	real = user + sys + 损耗

	损耗有很多，如IO平凡操作。


## 虚拟机ubuntu 挂载硬盘

	1. 先在虚拟机里进行设置要增加的硬盘大小。
	2. 进入ubuntu，在root用户下：
		1. fdisk -l	查看新加的盘符
		2. 使用：mkfs -t ext3 设备名称	
			1. 设备名称通过fdisk -l 可以查看
		3. 在home目录下，新建一个目录名称，如work
		4.  mount /dev/sdb /home/work 将硬盘挂载
			1.  /dev/sdb	是设备名称
		5.  df -l 可以查看刚挂载的硬盘


	实现开机自动挂载：
		修改/etc/fstab，增加下面这句：
			/dev/sdb /home/work ext3 defaults 0 0

		即可。
			/dev/sdb	是设备名称，用fdisk -l 可以查看。



## alias	

	在shell输入：alias 可以显示一些shell命令的 简写


## history
	
	history	-- 显示之前输入过的命令


	遍历历史记录：
	ctrl+p 可以调用之前输入的命令 是从最后输入的命令往上开始显示
	
	ctrl+n 从上往下 显示之前输入的命令



# 目录

	/bin: 存放的是二进制文件，是可执行程序，shell命令，可以直接运行	
	
	/dev: 驱动相关 在linux下一切皆文件：硬盘、显卡、显示器...
	
	/lib: 存放linux运行时需要加载的动态库 不要随便更改这个目录
	
	/mnt: 手动的挂载目录
	
	/media: 外设的自动挂载目录
	
	/root: linux的超级用户的 家目录
	
	/usr: unix system resoure
	
			头文件-stdio.h stdlib.h	/usr/include/stdio.h
			游戏
			用户安装的程序	/usr/local
	
	/etc: 存放 配置文件
	
		/etc/passwd	
		/etc/group
	
	/opt: 安装第三方应用程序
	
	/home: linuxa系统 所有用户的家目录
	
		用户家目录： /home/hxh
	
	/tmp: 存放 临时文件 关机之后当前目录会被清空

	/boot: 存放 开机启动项


## 目录

1.相对路径：从当前目录开始表示
		hxh/
		./hxh/

2.绝对路径：从根目录/开始表示的路径
		/home/hxh

3. ~: 表示用户的家目录
4. . 和 ..
	. 表示当前目录
	./ 表示当前目录下
	.. 表示当前目录上一级目录
	
5. hxh@hxh-virtual-machine:~/Documents$ 

		hxh: 当前登录的用户
		@: at,在
		hxh-virtual-machine： 安装时指定的主机名
		~：用户的家目录（宿主目录）
		/Documents: 当前用户的工作目录，就是在当前目录操作
		$: 当前用户为普通用户
		#: 显示当前用户为超级用户


## tree

	查看目录结构
	tree 查看当前目录
	tree dir 查看指定目录
	需要安装：sudo apt-get install tree

	tree命令行参数：
	
	-a 显示所有文件和目录。

	-A 使用ASNI绘图字符显示树状图而非以ASCII字符组合。

	-C 在文件和目录清单加上色彩，便于区分各种类型。

	-d 显示目录名称而非内容。

	-D 列出文件或目录的更改时间。

	-f 在每个文件或目录之前，显示完整的相对路径名称。

	-F 在执行文件，目录，Socket，符号连接，管道名称名称，各自加上"*","/","=","@","|"号。

	-g 列出文件或目录的所属群组名称，没有对应的名称时，则显示群组识别码。

	-i 不以阶梯状列出文件或目录名称。

	-I 不显示符合范本样式的文件或目录名称。

	-l 如遇到性质为符号连接的目录，直接列出该连接所指向的原始目录。

	-n 不在文件和目录清单加上色彩。

	-N 直接列出文件和目录名称，包括控制字符。

	-p 列出权限标示。

	-P 只显示符合范本样式的文件或目录名称。

	-q 用"?"号取代控制字符，列出文件和目录名称。

	-s 列出文件或目录大小。

	-t 用文件和目录的更改时间排序。

	-u 列出文件或目录的拥有者名称，没有对应的名称时，则显示用户识别码。

	-x 将范围局限在现行的文件系统中，若指定目录下的某些子目录，其存放于另一个文件系统上，则将该子目录予以排除在寻找范围外。


## ls  -- 查看文件或者目录

	
	ll 这是ls -alF 的别名

	-l
	drwxr-xr-x 25 hxh hxh 4096 11月  5 17:55 hxh

	第一个字符：文件的类型
		7种文件类型：
			- 普通文件
				- .txt 压缩包 可执行程序
			d 目录
			| 符号链接
			p 管道
			s 套接字
			c 字符设备
				键盘、鼠标
			b 块设备
				硬盘、U盘

	后面9个字符 3个一组，是权限
		第一组：rwx 是文件所有者的权限 读、写、执行
		第二组：r-x 是文件所属组用户的权限 -表示没有对应的操作，这里没有写操作
		第三组：r-x 其他人权限

	25：硬链接计数
	hxh: 文件所有者
	hxh: 文件所属组
	4096：文件大小	如果是目录，这个数永远是4096，只表示目录本身大小，不包括里面文件大小
		
	日期 和 文件名

	
	-a：显示所有档案及目录（ls内定将档案名或目录名称为“.”的视为影藏，不会列出）；

	-A：显示除影藏文件“.”和“..”以外的所有文件列表；

	-C：多列显示输出结果。这是默认选项；

	-l：与“-C”选项功能相反，所有输出信息用单列格式输出，不输出为多列；

	-F：在每个输出项后追加文件的类型标识符，具体含义：“*”表示具有可执行权限的普通文件，“/”表示目录，“@”表示符号链接，“|”表示命令管道FIFO，“=”表示sockets套接字。当文件为普通文件时，不输出任何标识符；

	-b：将文件中的不可输出的字符以反斜线“”加字符编码的方式输出；

	-c：与“-lt”选项连用时，按照文件状态时间排序输出目录内容，排序的依据是文件的索引节点中的ctime字段。与“-l”选项连用时，则排序的一句是文件的状态改变时间；

	-d：仅显示目录名，而不显示目录下的内容列表。显示符号链接文件本身，而不显示其所指向的目录列表；

	-f：此参数的效果和同时指定“aU”参数相同，并关闭“lst”参数的效果；

	-i：显示文件索引节点号（inode）。一个索引节点代表一个文件；

	--file-type：与“-F”选项的功能相同，但是不显示“*”；

	-k：以KB（千字节）为单位显示文件大小；

	-l：以长格式显示目录下的内容列表。输出的信息从左到右依次包括文件名，文件类型、权限模式、硬连接数、所有者、组、文件大小和文件的最后修改时间等；

	-m：用“,”号区隔每个文件和目录的名称；

	-n：以用户识别码和群组识别码替代其名称；

	-r：以文件名反序排列并输出目录内容列表；

	-s：显示文件和目录的大小，以区块为单位；

	-t：用文件和目录的更改时间排序；

	-L：如果遇到性质为符号链接的文件或目录，直接列出该链接所指向的原始文件或目录；

	-R：递归处理，将指定目录下的所有文件及子目录一并处理；

	--full-time：列出完整的日期与时间；

	--color[=WHEN]：使用不同的颜色高亮显示不同类型的。


## cd --切换目录

	a: cd 目录
	b: 怎么进入到家目录
		cd
		cd ~
		cd /home/hxh
	c: 在临近的两个目录直接切换 cd -
		最后两个相临的目录之间进行切换

## mkdir  --创建目录

	mkdir (选项)(参数)

	选项：
		-Z：设置安全上下文，当使用SELinux时有效；

		-m<目标属性>或--mode<目标属性>建立目录的同时设置目录的权限；

		-p或--parents 若所要建立目录的上层目录目前尚未建立，则会一并建立上层目录；

		--version 显示版本信息。

	参数：
	
		目录：指定要创建的目录列表，多个目录之间用空格隔开。

	例：
		1. 在目录/usr/meng下建立子目录test，并且只有文件主有读、写和执行权限，其他人无权访问

			mkdir -m 700 /usr/meng/test

		2. 在当前目录中建立bin和bin下的os_1目录，权限设置为文件主可读、写、执行，同组用户可读和执行，其他用户无权访问

			mkdir -p-m 750 bin/os_1

		3. 在当前目录，创建test目录，并在test目录下创建bb目录
			
			mkdir -p test/bb
			这样默认test目录和bb目录，所有人都有可读、写、执行权限 777


## pwd --显示当前的目录


## mkdir --创建目录

	mkdir 目录名
		-p 创建多级目录

	mkdir aa	创建aa目录
	mkdir aa/bb/cc -p 可以在没有bb目录的情况下创建cc目录


## touch	--创建文件

	touch 文件名--如果文件不存在，创建文件-空文件
				如果文件存在，更新文件创建时间


## rmdir -- 只能删除空目录

## rm --删除目录或者文件

	rm 文件名
	rm -r 目录名	-r 表示递归的方式处理
	注意问题：
		删除之后，很难恢复
	-i 表示需要用户确认是否删除

	rm a.txt	删除文件
	rm -r test	删除test目录以及目录里的所有文件

	

## cp --拷贝

	cp 要拷贝的文件 被拷贝的文件file	
		如果被拷贝的文件不存在，会被创建
		如果被拷贝的文件存在，则该文件会被覆盖，文件名没有变，是不是文件的内容会被覆盖。
	
	cp file dir 
		目录存在，则file文件被拷贝到dir目录
		目录不存在， 拷贝失败： 
			cp a.txt test/ 有/才是拷贝到目录，没有/表示文件拷贝到文件
			cp a.txt test	表示将a.txt 拷贝到文件test
	
	cp dir1（存在） dir2（存在）
		将dir1 拷贝到dir2里面
	
	cp dir1（存在） dir2（不存在） -r
		创建dir2，再将dir1中的内容拷贝到dir2中


## mv --改名或者移动 

 	mv file1 file2
	改名：
		mv file1(存在）file2（不存在）改文件名
		mv dir(存在） dir2（不存在）	改目录名
		mv file1（存在） file2（存在）将file1文覆盖file2文件

	移动：
		mv file（存在文件） dir（存在目录）	将file移动到dir中
		mv dir（存在目录） dir2（存在目录）	将dir目录移动到dir2中

		mv file(存在文件） dir(不存在目录）	出错，不能移动
		

## cat --查看文件内容

	cat 文件名	

	主要用在 在中端上 查看小文件的内容


## more --查看文件内容

	more 文件名

	可以查看大文件 通过按空格不断向下浏览
	回车--向下浏览一行
	空格--向下翻页
	q--退出


##  less --查看文件内容

	less 文件名

	向下滚动一行：
		回车
		ctrl+n

	向上滚动一行：
		ctrl+p

	向下翻页：
		空格
		pagedown
	
	向上翻页：
		pageup

	退出：
		q

## head --查看文件内容

	head 文件名

	默认查看文件的前 10 行

	head 文件名 -5 表示查看前 5 行


## tail  --查看文件内容


	tail 文件名
	tail -n 文件名	后n行 

	默认查看文件的后 10 行

	tail 文件名 -5 表示查看后 5 行



## 软硬链接 ln

	软链接 -- 相当于快捷方式
		ln -s 文件名  快捷方式名
			如： ln -s a.txt b.txt 	这样b.txt就是a.txt的快捷方式

		文件名 用绝对路径，就可以把 b.txt移动到任何地方都能访问到a.txt
		如： ln -s /home/hxh/a.txt b.txt
			b.txt的大小为 所存文件路径的文字的大小

	目录也可以创建软件链接和文件一样创建即可。


	硬链接 --不占用硬盘空间和原文件指向同一个inode节点
		ln 文件名 硬链接名字	--创建硬链接
			硬链接时，文件名可以不用绝对路径，相对路径也行。

	修改硬链接的文件，原文件也会被修改。


## linux 是通过 inode节点号 找到文件的


## chmod --修改文件或目录权限

	1. 文字设定法

		chmod who[+|-|=]mode 文件名
			who: u --user ,文件所有者
				 g -- group,文件所属组
				 o -- other, 其他人
				 a -- 表示所有人，就是3类人都有，默认是这样
			+、- 、=： + 表示增加权限，- 表示减少权限，= 表示设置权限

			mode:
				r: 读取、 w: 写、 x： 执行、 -： 没有任何权限

		如： chmod uo -rw a.txt	表示文件所有者和其他人都减去读和写 权限

			chmod u+r,g-r a.txt	表示文件所有者加上读权限，所有组减去读权限


	2. 数字设定法

		chmod [+|-|=] mode 文件名
			+/-/= 经常不用写
			mode:为数字 八进制数字
				r--4
				w--2
				x--1
				- -- 0	不对权限操作

		chmod 331 a.txt		表示文件所有者有写和执行权限，所属组也是写和执行权限，其他人只有执行权限

		chmod - 440 a.txt	表示给所有者和所属组减去读权限，其他人权限不变




## chown -- 修改文件所有者和所属组

	chown 新的所有者 文件名
	chown 新的所有者：新的所属组 文件名

	如：sudo chown my a.txt	将文件的所有者 改 为 my
		sudo chown hxh:pulse a.txt 将文件的所有者 改 为hxh，所属组改为 pulse

## chgrp	--修改文件的 所属组

	chgrp 新的所属组 文件名 
	
	如：chgrp mygroup a.txt	文件的所属组 改为  mygroup
	


## find --文件属性 查找

	查找方式：

	文件名

		find 查找的目录 -name "要查找的文件名" 

			find . -name "a.txt" 在当前目录下查找名叫a.txt的文件

			 find . -name "*.txt"	查找txt结尾的文件

	文件类型

		find 查找的目录 -type 文件类型
			
				文件类型：
					普通文件：	f
					目录：		d
					符号链接：	l	字符l
					管道：		p
					套接字：		s
					字符设备：	c
					块设备：		b

			find . -type d	表示查找目录文件
		


	文件大小
		
		find 查找的目录 -size 大小
			大小： - 表示小于
					+ 表示大于
			等于10k 就是 10k
			单位：
				k--小写
				M--大写

			如： find . -size -10M	表示查找小于10M的文件
				find . -size 10M	表示查找等于10M的文件

				大于10k小于 100k: find . -size +10k -size -100k
					

	按日期
		
		创建日期： -ctime -n/+n
			-n: n天以内		-1 表示1天以内的文件
			+n: n天以外 如：+3 表示3天前的文件		

			find . -ctime -1	

		修改日期：	-mtime -n/+n

		访问日期：  -atime -n/+n

			和上面的是一样的
			
			find . -mtime +2
			find . -atime -1


	深度

		-maxdepth n	-- n表示层数	表示搜索的最深层数为 n 层，就是n 层目录以下

			find ../ -maxdepth 3 -name "b.txt"

		-mindepth n -- 表示搜索最少要搜索 n 层，少于n层目录 的不用看

			find . -mindepth 2 -name "a.txt"

	
	高级查找

		例：查找指定目录，并列出该目录的详细信息
			--find ./ -type d -exec shell命令 (ls -l) {} \;

			如：find ../ -type d -maxdepth 2 -exec ls -l {} \;

			-- find ./ -type d -ok shell命令 {} \;

			-ok 相对比较安全  ok 需要用户确认

			-- find ./ -type d | xargs shell命令

				find ./ -type d | xargs ls -l	用管道速度相对快一些


		
		1.查找用户目录下大于100k，小于10M的文件，并将结果输出到文件
		find ~ -size +100k -size -10M | xargs ls -l > file.txt




## grep 根据文件内容查找

	grep -r(有目录需要加，一般加入即可) "查找的内容" 搜索的路径

	例：搜索家目录中带有hello字符串内容的 文件	是查找文件内容中有hello字符串的文件，不是文件名有hello的文件

		grep -r "hello" ~	

		grep -r "hello,world" ./ 搜索当前目录下 文件内容中有 hello,world 字符串的 文件



总结：find 搜索的路径 参数 搜索的内容	find是根据文件名字或者自身大小之类的查找，但是不能根据文件内部的内容来查找
	 grep 搜索的内容	参数  搜索的路径 	grep是根据文件内容来查找



## 常用压缩命令
	
	tar	--打包

		参数：
			c -创建压缩文件
			x -释放压缩文件
			v -打印提示信息（可以不写）
			f -指定压缩包名字
			z -使用gzip压缩文件	-xxx.tar.gz
			j -使用bzip2方式 压缩文件  -xxx.tar.bz2

		压缩：
			tar 参数 压缩包的名字 原材料就是要压缩的文件
				tar zcvf test.tar.gz test a.txt  可以压缩多个目录或文件
			

		解压缩：
			
			tar zxvf 要解压的文件
			tar zxvf 要解压的文件 -C 解压到对应目录	
				要解压到目录就要加入-C

				tar zxvf test.tar.gz -C bbb/


	压缩： tar 参数 压缩包名  原材料（可以是多个文件和目录）
	解压缩： tar 参数	 压缩包名 （参数 解压路径）



	rar	

		rar 需要安装
			sudo apt-get install rar

		压缩：
			rar a 压缩包名（不用指定后缀） 压缩内容

				-r 有目录时需要加参数 
		
		 rar a tt a.txt 
		 rar a tt a.txt bbb -r


		解压缩:
			rar x 压缩包名  --解压缩到当前目录

			rar x 压缩包名 解压目录

			rar x tt.rar		解压到当前目录
			rar x tt.rar bbb/	解缩到对应目录下

	

	zip/unzip

		压缩：
			zip 参数 压缩包名 原材料

				如果有目录加上： -r

			zip zz a.txt	-- 压缩为zz.zip
			zip as a.txt bb/ -r  --压缩文件和目录为as.zip

		解压缩：
			unzip 压缩包名 （-d 目录名）


## echo

	输出字符串或一些变量的值
	如：echo hello	输出字符串hello
		echo $PATH $表示变量，输出变量PATH的值


## 软件的安装和卸载

	1.在线安装 对应ubuntu系统

		安装：
			sudo apt-get install 安装包名字	如：sudo apt-get install tree 

			ubuntu16.04后 apt-get 简写为 apt: sudo apt install 安装包名


		卸载：
			sudo apt-get remove 软件名 或 
			sudo apt remove 软件名 --ubuntu16.4之后用这个命令

			软件名并不一定和安装包名是一样的，可以先搜索看看有没有这个软件名
			

		软件列表的更新：
			sudo apt-get update 或 
			sudo apt update --ubuntu16.4之后用这个命令


		清空缓存：
			sudo apt-get clean 或 
			sudo apt clean --ubuntu16.4之后用这个命令
			
			/var/cache/apt/archives		缓存的文件是在这个目录下

	2. 软件包的安装 ubuntu下 为 xx.deb

		安装：
			sudo dpkg -i xxx.deb	xxx.deb为安装包名

		卸载：
			sudo dpkg -r 软件的名字	软件名并不一定和安装包名一样，卸载前先查看一下


	3.源码安装
		查看源码目录下的一个readme 文件或README	文件里面有安装步骤


































		