
## u-boot.lds	在u-boot 当前目录下的这个文件

	u-boot.lds	是将编译好的文件 链接在一起，组成 u-boot.bin 文件

	OUTPUT_FORMAT("elf32-littlearm", "elf32-littlearm", "elf32-littlearm")
	OUTPUT_ARCH(arm)
	ENTRY(_start)
	SECTIONS
	{
	 . = 0x00000000;	// 0x00000000 +0xc3e00000
	 . = ALIGN(4);		// 以四字节对齐
	 .text :			// .text表示代码段
	 {
		// 先放cpu/arm_cortexa9/start 文件在 0x00 这个位置 依次放下面的文件
		// start.o 调用 lowlevel_init.o , lowlevel_init.o 调用  cpu_init.o
	  cpu/arm_cortexa9/start.o (.text)				
	  cpu/arm_cortexa9/s5pc210/cpu_init.o (.text)
	  board/samsung/smdkc210/lowlevel_init.o (.text)
	  common/ace_sha1.o (.text)
	  *(.text)
	 }
	 . = ALIGN(4);
	 .rodata : { *(SORT_BY_ALIGNMENT(SORT_BY_NAME(.rodata*))) }	// 所有文件的只读数据段
	 . = ALIGN(4);
	 .data : { *(.data) }	// 所有文件的数据段
	 . = ALIGN(4);
	 .got : { *(.got) }
	 __u_boot_cmd_start = .;		// uboot的命令段
	 .u_boot_cmd : { *(.u_boot_cmd) }
	 __u_boot_cmd_end = .;
	 . = ALIGN(4);
	 __bss_start = .;
	 .bss : { *(.bss) }			// 没有初始化的静态变量和全局变量
	 _end = .;
	}


