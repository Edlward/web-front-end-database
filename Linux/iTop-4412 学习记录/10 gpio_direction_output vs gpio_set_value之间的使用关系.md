
##　gpio_direction_output vs gpio_set_value之间的使用关系

	
	在linux驱动中常常会碰到gpio_set_value（port_num,0/1）或gpio_direction_output （port_num,0/1)
	
	  这两者有什么关系呢
	
	gpio_set_value（port_num,0/1） 一般只是在这个GPIO口的寄存器上写上某个值，至于这个端口是否设置为输出，它就管不了！
	
	而gpio_direction_output （port_num,0/1)，在某个GPIO口写上某个值之后，还会把这个端口设置为输出模式。
	
	 因此，有人也许就会建议，把gpio_set_value这个函数直接去掉不用，是否可以，显然是可以的。
	
	 但是为什么系统还要用呢，
	
	 我个人分析是，
	
	    系统开发人员在要结合这两者来使用，以便提高效率。
	
	   一般某个端口设置好了输入与输出模式后，最好不要经常变动。
	
	   首先要调用gpio_direction_output（），以后要设置高低电平时，直接使用gpio_set_value（）就可以了，这样可以省却再次调用设置输出模式的操作，从而提高运行效率！



