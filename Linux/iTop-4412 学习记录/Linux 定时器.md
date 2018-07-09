
## 主要内容：Linux-定时器


	1. Linux定时器基础知识
	
		1.1 定时器的使用范围（延后执行某个操作，定时查询某个状态；前提是对时间要求不高的地方）
		1.2 内核时间概念
		HZ： 系统时钟通过 CONFIG_HZ 来设置，范围是100-1000;HZ决定时钟中断发生的频率
		
	    内核的全局变量 jiffies：
	        记录内核自启动来的节拍数, 内核之启动以来，产生的中断数  
	    
		jiffies/HZ 内核自启动以来的秒数

		
	2.  内核定时器的例程
  
		结构体 timer_list
	    
		2.1 timer_list 参数 
			struct list_head entry 双向链表。
			unsigned long expires;超时时间。记录什么时候产生时钟中断。
			struct tvec_base *base;管理时钟的结构体
			void (*function)(unsigned long);时钟中断产生之后的动作
			unsigned long data;传递的参数
			
		2.2 双向链表
		platform_driver_register→driver_register
		→bus_add_driver→struct bus_type *bus
		→struct subsys_private *p
		→struct kset subsys
		→struct list_head list;
		
		2.3 mod_timer = del_timer(timer)；timer->expires = expires; add_timer(timer);


## 函数 setup_timer add_timer del_timer mod_timer


	初始化定时器
		setup_timer	这是个宏定义


	增加定时器
	
	void add_timer(struct timer_list * timer);
	
	
	删除定时器
	
	int del_timer(struct timer_list * timer);
	　　
	
	修改定时器的expire
	
	int mod_timer(struct timer_list *timer, unsigned long expires);
	
	mod_timer 相当于：
		del_timer(timer)；
		timer->expires = expires; 
		add_timer(timer);


	使用定时器的一般流程为：
	
	（1）timer、编写function；

	（2）通过setup_timer 为timer的expires、data、function赋值；

	（3）调用add_timer将timer加入列表；

	（4）在定时器到期时，function被执行；

	（5）在程序中涉及timer控制的地方适当地调用del_timer、mod_timer删除timer或修改timer的expires。
	


