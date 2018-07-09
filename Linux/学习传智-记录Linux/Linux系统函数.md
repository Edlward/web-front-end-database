
## open/close

	函数原型：
		int open(const char *pathname, int flags);
       	int open(const char *pathname, int flags, mode_t mode);


	返回值：文件描述符

	参数：
		flags:	打开文件的方式
			必选项： O_RDONLY,  O_WRONLY,  or  O_RDWR 其中一个即可
			可选项：
				创建文件：O_CREAT
					创建文件时检测文件是否存在：O_EXCL
						如果文件存在，返回 -1
						必须与O_CREAT一起使用

				追加文件：O_APPEND
				文件截断：O_TRUNC	就是清空文件
				
	O_NONBLOCK --以不可阻断的方式打开文件，也就是无论有无数据读取或等待，都会立即返回进程之中。

	O_NDELAY -- 同O_NONBLOCK。

	O_TRUNC --若文件存在并且以可写的方式打开时，此旗标会令文件长度清为0，而原来存于该文件的资料也会消失。

	O_NOCTTY -- 如果欲打开的文件为终端机设备时，则不会将该终端机当成进程控制终端机。


		mode:	文件权限
			mode & ~umask

				umask为：0002	所以权限最多是：775

## create


	函数原型：
		int creat (const char *pathname,mode_t mode);

	返回值：文件描述符

	函数也能打开一个文件，如果文件不存在，则创建它。和open一样，creat也在调用成功后返回一个文件描述符，如果失败，则设置errno变量并返回-1.

	create 等价于:

		open(pathname,O_CREAT | O_TRUNC | O_WRONLY,mode);


## read

	函数原型
		ssize_t read(int fd, void *buf, size_t count);


	参数：
		fd: open的返回值，文件描述符
		buf: 缓冲区，存储要读取的数据
		count: 缓冲区存储的最大字节数 sizeof(buf)


	返回值：
		-1： 失败
		成功：
			大于0的数，表示读取的字节数
			等于0，表示文件读完了



## write

		函数原型
		ssize_t write(int fd, const void *buf, size_t count);


	参数：
		fd: open的返回值，文件描述符
		buf: 缓冲区，存储要写到文件的数据
		count: 写入文件的有效字节数，strlen(buf)


	返回值：
		-1： 失败
		成功：
			大于0的数，表示写入的字节数
			



	例：有一个很大的文件，open读取出来，read到另一个文件中

	#include <stdio.h>
	#include <string.h>
	#include <stdlib.h>
	#include <unistd.h>
	#include <sys/types.h>
	#include <sys/stat.h>
	#include <fcntl.h>
	
	
	int main(int argc, const char* argv[])
	{
	
		int fd = open("main.c", O_RDWR);
		printf("fd = %d \n", fd);	// 其实要判断是否打开正确
		if( -1 == fd)
		{
			return;
		}
	
		// use to write
		int fd_w = open("tmpxx", O_WRONLY | O_CREAT, 0664);
		printf("fd_w = %d \n ", fd_w);
		if( -1 == fd_w)
		{
			close(fd);
			return;
		}	

		char buf[4096];
		int ret;
		int len = read(fd, buf, sizeof(buf)); 	
		while( len > 0 )
		{
			ret = write(fd_w, buf, len);		
			printf("ret = %d \n", ret );
	
			len = read(fd, buf, sizeof(buf)); 	
		}
	
		close(fd);
		close(fd_w);
	
		return 0;
	}


## perror -- 专门输出错误信息

	如： perror();	会输出对应的错误信息，错误信息 是由库中的errno整型值来确认的。error这个值在文件操作错误时，会由库设置成对应的值。

	perror("my_function");	先输出字符串，再输出错误信息。




## lseek
	
	函数原型：
		off_t lseek(int fd, off_t offset, int whence);
		whence:
			SEEK_SET
			SEEK_CUR
			SEEK_END	

	使用：
		a. 文件指针移动到头部
			lseek(fd, 0, SEEK_SET)
	
		b. 获取文件指针当前的位置：
			int len = lseek(fd, 0, SEEK_CUR);

		c. 获取文件长度：
			int len = lseek(fd, 0, SEEK_END);


	文件拓展：

		文件原来大小100k，拓展为1100k
			lseek(fd, 1000, SEEK_END);
			然后最后做一次写操作：
			write(fd, "a", 1); 随便写什么都可以。
	

## 阻塞和非阻塞

	阻塞读取终端：

		#include <unistd.h>
		#include <stdio.h>
		#include <stdlib.h>
		
		int main()
		{
			char buf[10];
			int ret;
			// 等待终端输入，会一直等，直到有输入。
			ret = read(STDIN_FILENO, buf, 10);
			if(-1 == ret)
			{
				printf("read error");
				exit(1);
			}
		
			write(STDOUT_FILENO, buf, 10);
		
			return 0;
		}


	阻塞和非阻塞是文件的属性

	普通文件如hello.c，默认是不阻塞

	终端设备： /dev/tty
		默认阻塞
		管道也是默认阻塞
		套接字默认也是阻塞



## stat/lstat 函数

	函数原型：	man 2 stat可以查看信息
		
       int stat(const char *path, struct stat *buf);
       int fstat(int fd, struct stat *buf);
       int lstat(const char *path, struct stat *buf);

	stat 和 lstat区别在于读取软链接时不同
		lstat 读取的是链接文件本身的属性
		stat 读取的是链接文件指向的文件的属性
			stat具有 穿透 功能

	fstat 和 stat 区别只是参数不同，fstat 需要 文件描述符

	
		例：

		#include <sys/types.h>
		#include <sys/stat.h>
		#include <unistd.h>
		#include <stdio.h>
		#include <string.h>
		#include <stdlib.h>
		
		int main()
		{
			struct stat st;
			// 检测test.txt文件
			int ret = stat("test.txt", &st);
			if(-1 == ret)
			{
				printf("stat error");
				exit(1);
			}
		
			// 打印一些信息
			printf("file size = %d \n", (int)st.st_size);
			// 文件类型 判断是不是普通文件
			if((st.st_mode & S_IFMT) == S_IFREG)
			{
				// 如果要判断多个文件类型，用switch--case
				printf("这个文件是普通文件 \n");
			}
		
			// 所有者对文件的权限
			if((st.st_mode & S_IRWXU) == S_IRUSR )
			{
				printf("文件所有者 有读权限 \n");
			}
		
			if((st.st_mode & S_IRWXU) == S_IWUSR )
			{
				printf("文件所有者 有写权限 \n");
			}
		
			if((st.st_mode & S_IRWXU) == S_IXUSR )
			{
				printf("文件所有者 有执行权限 \n");
			}
		
		
			return 0;
		}
		


## access	-- 测试当前文件是否具有某属性

	函数原型：
		int access(const char *pathname, int mode);

	
	参数：
		pathname: 文件名
	
		mode: 	检测类型
		
			W_OK	--是否有写权限
			R_OK	--是否有读权限
			X_OK	--是否有执行权限
			F_OK	--文件是否存在


	返回值：
		0  - 具有某些权限
		-1 - 没有某些权限



## chmod	--修改文件权限

	函数原型：
		 int chmod(const char *path, mode_t mode);
	
	参数：
		path: 文件名
		mode: 文件权限，八进制数	如： 666


## chown	--修改文件所有者和所属组 权限

	函数原型：
		int chown(const char *path, uid_t owner, gid_t group);


	参数：
		path: 文件路径，就是文件名
		owner: 整型值，用户ID
				查看 /etc/passwd	中有对应用户ID 
				hxh:x:1000:1000:hxh,,,:/home/hxh:/bin/bash
				第一个1000就是用户ID

		group: 整型值，用户组ID
			查看/etc/group 



## truncate		--修改文件大小

	函数原型：
		int truncate(const char *path, off_t length);
        int ftruncate(int fd, off_t length);


	参数：
		path: 文件名
		length: 文件的最终大小
			1.比原来小，删除后边的部分
			2.比原来大，向后拓展，填充占位符\0





## rename		--文件重命名

	函数原型：
		int rename(const char *oldpath, const char *newpath);



## chdir		--修改当前进程（应用程序）的路径	相当于shell cd
	
	函数原型：
		 int chdir(const char *path);

	参数：
		path:	切换的路径


	是改变应用程序的路径，不是改变当前shell的路径




## getcwd		--获取当前路径	相当于shell pwd

	函数原型：
		char *getcwd(char *buf, size_t size);

	参数：
		buf:	缓冲区，缓存当前的路径
		size:	缓冲区大小

	返回值：
		NULL：	获取错误
		正确：	当前的工作目录



## mkdir		--创建目录

	函数原型：
		 int mkdir(const char *pathname, mode_t mode);

	参数：
		pathname:	目录名
		mode:	目录的权限，八进制，如：774
			mode & ~umask	最后两位始终是 0


## rmdir		--删除空目录

	函数原型：
		 int rmdir(const char *pathname);



## opendir		--打开一个目录	是c库函数 man 3 opendir

	函数原型：
		DIR *opendir(const char *name);

	参数：
		name: 目录名

	返回值：
		指向目录的指针


	读取目录：
		struct dirent * readdir(DIR* dirp);


	关闭目录：
		int closedir(DIR *dirp);


	例：	统计对应目录文件个数
	#include <sys/types.h>
	#include <sys/stat.h>
	#include <unistd.h>
	#include <stdio.h>
	#include <string.h>
	#include <stdlib.h>
	#include <dirent.h>
	
	int get_file_num(const char * root)
	{
	
		int total = 0;
	
		DIR* dir = NULL;
		dir = opendir(root);
		if(NULL == dir)
		{
			perror("opendir error");
			exit(1);
		}
		
		struct dirent* ptr = NULL;
	
		while(  (ptr = readdir(dir) ) != NULL )
		{
			// . .. 
			if(strcmp(".", ptr->d_name) == 0 || strcmp("..", ptr->d_name) == 0 )
			{
				continue;
			}
	
			// normal file
			if(ptr->d_type == DT_REG)
			{
				total++;
			}
	
	
			// dir
			if(ptr->d_type == DT_DIR)
			{
				char path[1024] = { 0 };
				sprintf(path, "%s/%s", root, ptr->d_name);
				total += get_file_num(path);
			}
	
	
		}
	
		closedir(dir);
		
		return total;
	}
	
	
	int main(int argc, const char* argv[])
	{
	
		// 目录通过 传参方式，传入
		if(argc < 2)
		{
			printf("./a.out path \n");
			exit(1);
		}
	
		int total = get_file_num(argv[1]);
		printf("%s dir file num: %d \n", argv[1], total);
	
		return 0;
	}
	

## dup		--复制文件 描述符

	函数原型：
		int dup(int oldfd);
       	int dup2(int oldfd, int newfd);

	参数：
		oldfd:	要复制的文件描述符
	
	返回值：
		新的文件描述符，是取最小的没被占用的文件描述符

	函数成功：两个文件描述符指向同一个文件

	dup2 可以指定新的 文件描述符 

	dup2(fd1, 10);	表示新的文件描述符为 10	并且，10指向fd1所指向的文件
	
	参数：
		oldfd:  
		newfd:
			假设newfd已经指向一个文件，首先断开与那个文件的链接，close文件，newfd指向oldfd指向的文件
				这种情况也叫文件描述符的重定向
			
			newfd没有被占用，newfd直接指向oldfd指向的文件 

			oldfd和newfd指向同一个文件，函数不作任何处理。


	虽然可以复制一个文件描述符，让一个文件有两个文件操作符，但是文件内部的文件指针只有一个，是共用一个，就像一个文件操作读取了文件，读完了，另一个也想要读取文件，就要将文件指针移到开头，才能读取数据。


## fcntl		--改变已经打开的文件的属性，这是个参数可变的函数

	复制一个已有的文件描述符：
		int ret = fcntl(fd, F_DUPFD);
			相当于 dup(old);


	获取/设置文件的状态
		就是设置 open 的flags参数

		1.获取文件的状态标识：	就是open中flags参数的值
			1. int flag = fcntl(fd, F_GETFL);

		2. 设置文件的状态标识
			1. flag = flag | O_APPEND;
			2. fcntl(fd, F_SETFL, flag);



	可以更新的几个标识：O_APPEND、O_NONBLOCK
	
		1. 错误的操作为文件加O_RDWR之类的。



## chmod	--改变文件权限

	函数原型：
		int chmod(const char *path, mode_t mode);
       	int fchmod(int fd, mode_t mode);

		
		参数：
			path：文件路径。
			fd: 为open打开的文件的返回值。
			mode：直接使用数字即可。 这个数要写作八进制，如：0777

		返回值：
			成功返回0，错误返回-1。


## mkdir	--创建目录

	函数原型：
		int mkdir(const char *pathname, mode_t mode);

	参数：
		pathname: 文件路径
		mode：文件的权限，如0777，这个数也要写作八进制数。

	返回值：
		成功返回0，错误返回-1。



	删除目录用rmdir	--但是只能删除空目录
		
		int rmdir(const char *pathname);

		– 参数*pathname：文件和目录的路径

		– 返回值：成功返回0，错误返回-1



## opendir/closedir		--打开/关闭 目录

	DIR *opendir(const char *name);

		参数：
			目录的路径。

		返回值：
			成功返回指向目录流的指针，错误返回NULL

	int closedir(DIR *dirp);
	
	参数：
		opendir 返回的dir 指针

	返回值：
		成功返回0， 失败返回-1


## readdir		--存储获取的目录内容

	struct dirent *readdir(DIR *dirp);

		参数：为opendir打开文件后返回的值

		返回值：
			NULL：获取目录内容失败或者目录内容已经全部读取完成 
			成功：目录的书信息，记录在dirent结构体中

		结构体：
		Struct dirent
		{
		
		        ino_t     d_ino;        //此目录进入点的inode
		
		        ff_t      d_off;        //目录文件开头到此目录进入点的位移
		
		        signed short int   d_reclem;    //_name的长度，不包含NULL字符
		
		        unsigned  char   d_type;        // d_name所指的文件类型
		
		        char d_name[256];            	//文件名
		
		};

		常用d_name 文件类型和 d_name 文件名

		d_type 表示档案类型：

			enum
			{
			     DT_UNKNOWN = 0, 	// 未知的类型
			
			     DT_FIFO = 1, 		// 命名管道或FIFO
			
			     DT_CHR = 2, 		// 字符设备文件
			
			     DT_DIR = 4, 		// 普通目录
			
			     DT_BLK = 6, 		// 块设备文件
			
			     DT_REG = 8, 		// 普通文件
			
			     DT_LNK = 10, 		// 链接文件
			
			     DT_SOCK = 12, 		// 本地套接口
			
			     DT_WHT = 14 
			}; 
					

	循环读取文件函数：

	#include <stdio.h>  
	#include <string.h> 
	#include <stdlib.h>  
	#include <dirent.h>  
	#include <sys/stat.h>  
	#include <unistd.h>  
	#include <sys/types.h> 
	
	void listDir(const char *path)  //main函数的argv[1] char * 作为 所需要遍历的路径 传参数给listDir   
	{  
	        DIR              *pDir ;  //定义一个DIR类的指针
	        struct dirent    *ent  ;   //定义一个结构体 dirent的指针，dirent结构体见上
	        int               i=0  ;  
	        char              childpath[512];  //定义一个字符数组，用来存放读取的路径
	  
	        pDir=opendir(path); //  opendir方法打开path目录，并将地址付给pDir指针
	        memset(childpath,0,sizeof(childpath)); //将字符数组childpath的数组元素全部置零 
	  
	  
	        while((ent=readdir(pDir))!=NULL) //读取pDir打开的目录，并赋值给ent, 同时判断是否目录为空，不为空则执行循环体  
	        {  
	  
	                if(ent->d_type & DT_DIR)  //读取 打开目录的文件类型 并与 DT_DIR进行位与运算操作，即如果读取的d_type类型为DT_DIR (=4 表示读取的为目录)
	                {  
	  
	                        if(strcmp(ent->d_name,".")==0 || strcmp(ent->d_name,"..")==0)  
	                                    //如果读取的d_name为 . 或者.. 表示读取的是当前目录符和上一目录符, 用contiue跳过，不进行下面的输出  
	                                continue;  
	  
	                        sprintf(childpath,"%s/%s",path,ent->d_name);  //如果非. ..则将 路径 和 文件名d_name 付给childpath, 并在下一行prinf输出
	                        printf("%s\n",childpath);  
	  
	                        listDir(childpath);  //递归读取下层的字目录内容， 因为是递归，所以从外往里逐次输出所有目录（路径+目录名），
	                                         //然后才在else中由内往外逐次输出所有文件名
	  
	                }  
	              else  //如果读取的d_type类型不是 DT_DIR, 即读取的不是目录，而是文件，则直接输出 d_name, 即输出文件名
	              {
					printf("%s/%s \n", path, ent->d_name);
	              }
	        }  
	}  



## link		--给文件取别名，相当于硬链接

	int link(const char *oldpath,const char *newpath);

	作用：
		link()函数为一个已经存在的文件创建一个新的链接（也就是通常所说的“硬链接”）
	
		如果新文件已经存在，它将不会被重写

		新的名字可以替代旧的名字做任何操作，这些名字都指向同一个文件（并且也有相同的权限和拥有者）



	返回值：

      一旦成功，返回0，一旦错误，返回-1。并且erron被设置了结果



## symlink	-- 相当于软链接，和快捷方式差不多

	int symlink(const char *oldpath, const char *newpath);

	参数：
		oldpath：已有的文件路径
		newpath：新建的符号链接文件路径

	返回值：
	
	      一旦成功，返回0，一旦错误，返回-1。　并且erron被设置了结果


##　unlink		--解除链接函数

	int unlink(const char *pathname);

	参数:
		pathname：链接文件的路径

	返回值：
		成功返回0，错误返回-1


	pathname 指向软链接，删除软链接；指向硬链接，相当于删除文件









