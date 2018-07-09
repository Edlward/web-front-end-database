
## 4412 内adc

	4412内部就只有 ADC0 ~ ADC3 4个通道的ADC

	Xadc1AIN_0
	Xadc1AIN_1
	Xadc1AIN_2
	Xadc1AIN_3

	注册一个ADC客户
	// is_ts为1表示是触摸屏，为0表示不是触摸屏
	adcdev.client = s3c_adc_register(dev, NULL, NULL, 0);

	// 第二个参数表示选择第几个ADC通道
	// 返回值，就是ADC的读取值是 12位数的。 4096最大
	// 这个函数默认是读取10次ADC取平均值了
	ret = s3c_adc_read(adcdev.client, adcdev.channel);
	
	// 最后一个值为采样次数	这个函数是被上面这个函数调用了
	ret = s3c_adc_start(adcdev.client, adcdev.channel， 2);


	ADC调用过程：首先调用s3c_adc_register函数注册一个ADC客户，然后调用 s3c_adc_read 函数读取ADC转换的数据，在读取的过程中，系统会启动ADC中断来进行ADC转换。ADC初始化在内核 adc.c 中，不用自己写。


