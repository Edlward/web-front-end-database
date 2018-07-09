
## 本期介绍如何自己实现物理地址到虚拟地址的转化

	读写GPIO，控制GPIO的寄存器都是使用系统做好的虚拟地址
	
		iounmap和ioremap函数可以实现物理地址到虚拟地址的转化
	
	1.硬件
		KP_COL0 -> GPL2_0
		datasheet物理地址：
		GPL2CON = 0x1100_0000+0x0100 = 0x11000000+0x0100=0x11000100
		GPL2DAT = 0x11000104
		GPL2PUD = 0x11000108

		寄存器不一定都是32位的，也有16位和8位
		
	2.软件
	
	3.编译测试
		加载驱动，小灯灭
		卸载驱动，小灯亮


	4. 这种写法和单片机写法差不多，是直接寄存器操作。


##　ioremap函数

     	ioremap宏定义在asm/io.h内：

		#define ioremap(cookie,size)      __ioremap(cookie,size,0)
		
		__ioremap函数原型为(arm/mm/ioremap.c)：
		
		void __iomem * __ioremap(unsigned long phys_addr, size_t size, unsigned long flags);
		
		参数：
		
		phys_addr：要映射的起始的IO地址
		
		size：要映射的空间的大小	想要用多少就可以映射多少个
		
		flags：要映射的IO空间和权限有关的标志
		
		该函数返回映射后的内核虚拟地址(3G-4G). 接着便可以通过读写该返回的内核虚拟地址去访问之这段I/O内存资源。


## iounmap函数

    iounmap函数用于取消ioremap（）所做的映射，原型如下：

     void iounmap(void * addr);



	例：
		
		#include <linux/init.h>
		#include <linux/module.h>
		#include <asm/io.h>
		
		MODULE_LICENSE("Dual BSD/GPL");
		MODULE_AUTHOR("TOPEET");
		
		//用于存放虚拟地址和物理地址
		volatile unsigned long virt_addr, phys_addr;
		//用户存放三个寄存器的地址
		volatile unsigned long *GPL2CON,*GPL2DAT,*GPL2PUD;
		
		void gpl2_device_init(void)
		{
			//物理地址起始地址0x11000100→0x11000108
			phys_addr = 0x11000100;
		    
			//0x11000100是GPL2CON的物理地址
			virt_addr =(unsigned long)ioremap(phys_addr, 0x10);
		    
			//指定需要操作的寄存器地址
			GPL2CON = (unsigned long *)(virt_addr+0x00);
			GPL2DAT = (unsigned long *)(virt_addr+0x04);
			GPL2PUD	=(unsigned long *)(virt_addr+0x08);
		}
		
		//配置开发板的GPIO寄存器
		void gpl2_configure(void)
		{
			//配置为输出模式。bit3 bit2 bit1设置为0，bit0设置为1
			*GPL2CON  &= 0xfffffff1;
			*GPL2CON  |= 0x00000001;
		    
			//GPL2PUD寄存器，bit[0:1]设为0x03,上拉模式
			*GPL2PUD |=0x0003;
		}
		
		void gpl2_on(void)//点亮led灯
		{
			*GPL2DAT |= 0x01;   // GPL2DAT 8位寄存器
		}
		
		void gpl2_off(void)//灭掉led
		{
			*GPL2DAT &= 0xfe;
		}
		
		static int led_gpl2_init(void)
		{
			printk("init!\n");
			gpl2_device_init();//实现IO内存的映射
			gpl2_configure();//配置GPL2为输出模式
			gpl2_on();
			printk("led gpl2 open\n");
			return 0;
		}
		
		
		static void led_gpl2_exit(void)
		{
			printk("exit!\n");
			gpl2_off();
			
			printk("led gpl2 close\n");
		}
		
		module_init(led_gpl2_init);
		module_exit(led_gpl2_exit);


















