
## SSH登陆

	1. 安装ssh: sudo apt-get install ssh
	2. 修改 /etc/ssh/sshd_config
		将 PermitRootLogin without-password  释放
		然后 加入一行： PermitRootLogin yes 

		# Authentication:  
		LoginGraceTime 120  
		#PermitRootLogin without-password  
		PermitRootLogin yes  
		StrictModes yes  

		重启SSH 服务： service ssh restart

