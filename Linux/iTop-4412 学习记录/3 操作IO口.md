
## 操作IO口

	对宏定义EXYNOS4_GPL2(0)的操作就是对4412芯片管脚   GPL2-0寄存器的操作


## 对应头文件位置


	Linux中申请GPIO的头文件
		1. include/linux/gpio.h
	
		2. linuxGPIO申请函数和赋值函数
			1. gpio_request()
			1. gpio_set_value()

	三星平台的GPIO配置函数头文件
		1. arch/arm/plat-samsung/include/plat/gpio-cfg.h
		2. 包括三星所有处理器的配置函数
	
		3. 三星平台配置GPIO函数
			1. s3c_gpio_cfgpin()
			2. GPIO配置输出模式的宏变量: S3C_GPIO_OUTPUT
			

	三星平台EXYNOS系列平台，GPIO配置参数宏定义头文件
		1. arch/arm/mach-exynos/include/mach/gpio.h
		2. GPIO管脚拉高拉低配置参数等等
		3. 配置参数的宏定义应该在arch/arm/plat-samsung/include/plat/gpio-cfg.h文件中
		

	三星平台4412平台，GPIO宏定义头文件。已经包含在头文件gpio.h中
		1. arch/arm/mach-exynos/include/mach/gpio-exynos4.h
		2. 包括4412处理器所有的GPIO的宏定义


	例：

		// 定义LED引脚
		#define LED2_PIN    EXYNOS4_GPL2(0)

		/* 申请资源 */
		ret = gpio_request(LED2_PIN, "LEDS");	// LEDS是说明引用作用，给写程序看的
		if(ret < 0){    // 申请失败
			printk(KERN_EMERG "gpio_request EXYNOS4_GPL2(0) failed!\n");
			
	        return ret;
		}
		
	    /* GPIO配置输出模式 */
		s3c_gpio_cfgpin(LED2_PIN, S3C_GPIO_OUTPUT);
		
	    /* 初始化灯灭 */
		gpio_set_value(LED2_PIN, 0);

