
## 知识点:

	1. 操作系统分层的概念
	
		Windows：
			bios：	这个是固化在主板上的
			内核模式：	硬件初始化、API初始化
			用户模式：
			用户程序：		QQ之类的应用程序

		linux：
			bootloader→内核→文件系统→用户程序
	
			bootloader：		如4412中的u-boot就是一种bootloader
			内核：			linux内核
			文件系统：		linux一定要有文件系统
			用户程序

	2. bootboader种类介绍
		
		U-boot是最通用的 bootboader。（210，4412等等）
		vivi 针对三星的ARM来定制2440上有用到

	3. 4412休眠问题
			
			休眠重新唤醒时，可以直接跳过uboot，直接去运行系统

	4. nandflash中的ECC
	
		工艺水平决定，nandflash读写出错。
		ECC是纠错的算法。分为软件纠错和CPU硬件纠错。
		4412有1-16bit的纠错能力。
		tf卡：ECC纠错控制器+存储器
		eMMC：ECC纠错控制器+MMC（存储器）

	5. 为什么从4412开始就不需要jtag了

		4412的iROM支持TF。tf卡是移动设备。
		jtag在老的设备中，它是用来第一次烧写uboot的。
		
	6. BL1 是三星提供的二进制文件，没有源码。


	IROM--->BL1----->BL2---->uboot---->linux内核

## 做了哪些事情：

	1.解压的这些文件是做什么的
	2.编译
	3.烧写（拨码开关）
	4.运行uboot

## 问题小结

	疑问1：uboot源码等文件做什么，有什么用？
	疑问2：编译的过程怎么回事？
		疑问2.1：编译脚本build_uboot.sh
		疑问2.2：配置mkconfig文件做了哪些事情
			创建平台相关的链接文件
			创建config.mk
			创建config.h文件
		疑问2.3Makefile文件分析
		疑问2.4uboot镜像的组成
			BL1+BL2+BL2补全+u-boot.bin+补全+TZSW
	疑问3：烧写是怎么实现的？
	疑问4：tf卡启动和fastboot到底怎么回事？
	疑问5：uboot启动会做哪些事情
		5.1uboot启动之后运行的第一个文件
	u-boot.lds分析。第一个执行的文件是cpu/arm_cortexa9/start

## 问题：为什么需要 uboot

	分层，便于移植。 uboot中是硬件相关的东西。



## 带着疑问去看：Datasheet中关于uboot的部分

	uboot启动会做的事情：

	iROM: 简短的代码，在4412芯片上的内存存储器中存放。 64KB大小。不能改。
		
	BL1：First boot loader，它们在扩展存储器上，如EMMC
		BL1是三星提供的，不开源

	BL2：Second boot loader
		不由三星提供。有代码。

	有用的知识：
		OM（拨码开关）是由iROM控制的，应该是iROM根据OM引脚决定从哪加载程序。
		BL1需要iROM中的代码去校验


	BL1、BL2以及uboot是什么关系

		uboot镜像=BL1+BL2+uboot+TZSW
		获得信息：
		BL1=15k
		BL2=16K
		U-boot.bin=328k		这个中会填充很多0 ，大概 83K， 从 0x45000--0x59c00
		TZSW<156k


## 通过iROM去解决“问题4”的部分

	Android_Exynos4412_iROM_Secure_Booting_Guide_Ver.1.00.00


## iROM做了哪些事情

	关掉看门狗Watchdog，关掉中断IRQ，关掉内存管理单元MMU...

	根据OM拨码开关的值，决定从哪加载程序，如OM值为b10


## 问题4.1：OM拨码开关在哪里起作用

	答案:iROM中


## 问题4.2：OM拨码开关是怎么对应的呢

	以tf卡为例。TF卡启动，拨码开关要设置为10
	原理图实际：XOM2 = 1；XOM3=XOM5=0; XOM6=0;XOM1=0;XOM4=0
	发现它们是对应的，OM: 000010	--> SDMMC (Ch2)就是内部硬件MMC2
	就是TF卡的硬件接到 Xmmc2DATA2 这几个引脚。


## 问题4.3tf卡烧写以及fastboot烧写，它们的命令是怎么来的？

	答案：
		三星原厂资料tc4
	文档名称：
		SEC_Exynos4x12_[SSCR][TC4]ICS_Installation_Guide_RTM1.0.2


## 疑问：-Ttext = 0xc3e00000 	64M

	定义在board/samsung/smdkc210/config.mk文件



## uboot	内部文件简单说明
		
		1.all00_padding.bin 用0来补全。

		2.api 常用的接口

		3.board  常用的主板文件。只需要留下samsung/smdkc210目录

		4.build_uboot.sh 编译脚本

		5.common 通用的文件,与架构无关的文件

		6.config.mk 配置文件，后面详细分析

		7.cpu -->arm_cortexa9保留，其它去掉 

		8.CREDITS工作人员名单

		9.disk 实现磁盘分区的接口

		10.doc 说明的文档

		11.drivers uboot中的驱动，mmc，看门狗，时钟等等

		12.E4212 E4412_N.bl1.bin

		13.examples 例程

		14.fs 文件系统。ext2是一种文件系统。

		15.include 头文件
		 rm -rf asm-avr32 asm-blackfin asm-i386 asm-m68k asm-microblazeasm-mips asm-nios asm-nios2 asm-ppc asm-sh asm-sparc

		16.lib* 留下lib_arm，libfdt，lib_generic
		rm -rf lib_avr32 lib_blackfin lib_i386 lib_m68k lib_microblaze lib_mips lib_nios lib_nios2 lib_ppc lib_sh lib_sparc
		
		17.MAINTAINERS MAKEALL不用管。Makefile很重要，mkconfig配置文件
		详细分析。mkbl2 编译的时候看一下。

		18.onenand_ipl和nand_spl没用

		19.net网络驱动

		20.paddingaa补丁

		21.post 自检

		22.rules.mk脚本编译的说明.readme.txt,README说明文档

		23.sdfuse和sdfuse_q sd卡烧写相关

		24.tc4_cmm.cmm和uboot_readme.txt文件，三星tc4开发板的文档。

		25.tools工具 编译烧写等等工具
		
		26.CodeSign4SecureBoot_POP和CodeSign4SecureBoot_SCP
			SCP和POP，uboot中和安全相关的加密文文件
		



