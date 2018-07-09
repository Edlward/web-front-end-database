
## linux 网络相关


	1. 安装 xinetd: sudo apt-get install xinetd

	2. 判断主机是否安装了xinetd: sudo aptitude show xinetd

	3. 配置 xinetd	
		1. 要3个统一 文件名和service 后面的名字 以及 /home/hxh/这个名字
	
		2. 在/etc/xinetd.d 目录下创建一个文件，如： xhttp
	
		3. 修改 xhttp 文件： 内容如下

		service xhttp
		{
			socket_type	= stream
			protocol	= tcp
			wait		= no
			user		= hxh
			server		= /home/hxh/xhttp
			server_args	= /home/hxh/dir
			disable		= no
			flags		= IPv4
		}

		server: 执行的完整路径 xhttp这个文件和当前这个文件是不一样的，
				要自己写或者用别人写好的。
		server_args: 资源所在的路径


	4. sudo vim /etc/services	向其中加入端口号： 如 9527
		 
		 xhttp       9527/tcp
		 xhttp       9527/udp


	5. 重启 xinetd

		sudo service xinetd restart 


	6. 查看是否运行xinetd

		ps aux ｜ grep xinetd


	7. 127.0.0.1:9527 查看是否有资源


