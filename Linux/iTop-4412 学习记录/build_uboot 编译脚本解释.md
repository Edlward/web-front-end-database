	
## uboot 编译命令

	./build_uboot.sh SCP_1GDDR
	后面这个参数，根据核心板而定

	编译完成后，在当前文件夹下生成了： u-boot-iTOP-4412.bin 文件。
	生成的文件“u-boot-iTOP-4412.bin”文件就是 SCP 1G 内存核心板对应的 uboot 镜像文件。

## build_uboot.sh 简单注释

	#!/bin/sh	这个是一定要的， #表示 注释
	
	// if 开始 fi  结束 
	if [ -z $1 ] 	// 脚本的条件判断，判断是否有参数传进去，没有参数，则打印退出。
	then	
	   echo "!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!"
	   echo "Please use correct make config.for example make SCP_1GDDR for SCP 1G DDR CoreBoard linux,android OS"
	   echo "!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!"
	   exit 0
	fi
	// 条件判断 $1 为这个脚本编译时的第一个参数 这里表示对传入的参数判断
	if   [ "$1" = "SCP_1GDDR" ] ||   [ "$1" = "SCP_2GDDR" ] || [ "$1" = "SCP_1GDDR_Ubuntu" ] ||   [ "$1" = "SCP_2GDDR_Ubuntu" ]
	then 
	      sec_path="../CodeSign4SecureBoot_SCP/"	// 设置这个目录路径
	      CoreBoard_type="SCP"
	
	elif [ "$1" = "POP_1GDDR" ] || [ "$1" = "POP_1GDDR_Ubuntu" ]
	then
	      sec_path="../CodeSign4SecureBoot_POP/"
	      CoreBoard_type="POP"
	
	elif [ "$1" = "POP_2GDDR" ] ||  [ "$1" = "POP_2GDDR_Ubuntu" ]
	then
	     sec_path="../CodeSign4SecureBoot_POP/"
	     CoreBoard_type="POP2G"
	else
	      echo "make config error,please use correct params......"
	      exit 0
	fi
	
	// CPU核数
	CPU_JOB_NUM=$(grep processor /proc/cpuinfo | awk '{field=$NF};END{print field+1}')
	ROOT_DIR=$(pwd)		// ROOT_DIR=CUR_DIR等于当前目录pwd
	CUR_DIR=${ROOT_DIR##*/}
	
	
	
	#clean
	make distclean	// 设置distclean参数
	
	#rm link file	// 删除两个文件
	rm ${ROOT_DIR}/board/samsung/smdkc210/lowlevel_init.S	
	rm ${ROOT_DIR}/cpu/arm_cortexa9/s5pc210/cpu_init.S
	
	case "$1" in	// clean相关
		clean)
			echo make clean
			make mrproper
			;;
		*)
				
			if [ ! -d $sec_path ]	// 这个值在上面设置 需要../CodeSign4SecureBoot_SCP/这个目录，没有就退出
			then
				echo "**********************************************"
				echo "[ERR]please get the CodeSign4SecureBoot first"
				echo "**********************************************"
				return
			fi
					// 条件判断，然后执行	SCP_1GDDR就对应第一个if条件
	                if [ "$1" = "SCP_1GDDR" ]
	                then
	                	make itop_4412_android_config_scp_1GDDR
	
	                elif [ "$1" = "SCP_2GDDR" ]
	                then
	                       make itop_4412_android_config_scp_2GDDR
	
	                elif [ "$1" = "POP_1GDDR" ]
	                then
	                       make itop_4412_android_config_pop_1GDDR
	
	                elif [ "$1" = "POP_2GDDR" ]
	                then
	                       make itop_4412_android_config_pop_2GDDR
	
	                elif [ "$1" = "SCP_1GDDR_Ubuntu" ]	
	                then
	                       make itop_4412_ubuntu_config_scp_1GDDR
	
	                elif [ "$1" = "SCP_2GDDR_Ubuntu" ]
	                then
	                       make itop_4412_ubuntu_config_scp_2GDDR
	
	                elif [ "$1" = "POP_1GDDR_Ubuntu" ]
	                then
	                       make itop_4412_ubuntu_config_pop_1GDDR
	
	                elif [ "$1" = "POP_2GDDR_Ubuntu" ]
	                then
	                       make itop_4412_ubuntu_config_pop_2GDDR
			fi	
			
			make -j$CPU_JOB_NUM	// 4核机器 make -j4 为多核编译
			
			if [ ! -f checksum_bl2_14k.bin ]	// 判断checksum_bl2_14k.bin是否存在
			then
				echo "!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!"
				echo "There are some error(s) while building uboot, please use command make to check."
				echo "!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!"
				exit 0
			fi

			// 拷贝文件checksum_bl2_14k.bin和u-boot.bin
			cp -rf checksum_bl2_14k.bin $sec_path
			cp -rf u-boot.bin $sec_path
			rm checksum_bl2_14k.bin			// 删除checksum_bl2_14k.bin
			
			cd $sec_path	// 进入../CodeSign4SecureBoot_SCP/目录
			#./codesigner_v21 -v2.1 checksum_bl2_14k.bin BL2.bin.signed.4412 Exynos4412_V21.prv -STAGE2
			
			# gernerate the uboot bin file support trust zone
			#cat E4412.S.BL1.SSCR.EVT1.1.bin E4412.BL2.TZ.SSCR.EVT1.1.bin all00_padding.bin u-boot.bin E4412.TZ.SSCR.EVT1.1.bin > u-boot-iTOP-4412.bin
	
					// SCP核心板 CoreBoard_type值为 SCP
	                if  [ "$CoreBoard_type" = "SCP" ]
	                then	// cat生成u-boot-iTOP-4412.bin镜像
			        cat E4412_N.bl1.SCP2G.bin bl2.bin all00_padding.bin u-boot.bin tzsw_SMDK4412_SCP_2GB.bin > u-boot-iTOP-4412.bin
	
	                elif [ "$CoreBoard_type" = "POP" ]
	                then
	                   cat E4412.S.BL1.SSCR.EVT1.1.bin E4412.BL2.TZ.SSCR.EVT1.1.bin all00_padding.bin u-boot.bin E4412.TZ.SSCR.EVT1.1.bin > u-boot-iTOP-4412.bin
	
	                elif [ "$CoreBoard_type" = "POP2G"  ]
	                then
	                   cat bl2.bin u-boot.bin E4412.TZ.SSCR.EVT1.1.bin > u-boot-iTOP-4412.bin
	
	                else
	                   echo  "make uboot image error......" 
	                fi
			// 拷贝到编译目录
			mv u-boot-iTOP-4412.bin $ROOT_DIR

			// 删除文件checksum_bl2_14k.bin和u-boot.bin
			rm checksum_bl2_14k.bin
			#rm BL2.bin.signed.4412
			rm u-boot.bin
	
			echo 
			echo 
			;;
			
	esac
	

## 	uboot文件组成	
	
	这个是对应 SCP：

	E4412_N.bl1.SCP2G.bin  	15K
	bl2.bin 				14K
	all00_padding.bin 		2K    14K+2K=16K	这个文件是很多0，是进行填充的
	u-boot.bin 328K
	tzsw_SMDK4412_SCP_2GB.bin  155k <=156K	由这几个生成 uboot文件
	
	> u-boot-iTOP-4412.bin // 这个就是生成的uboot文件


	pop的uboot组成：
	bl1 =8K
	bl2 = 16K
	u-boot.bin 328K
	tzsw_SMDK4412_SCP_2GB.bin <=87k










