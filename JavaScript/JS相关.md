
## JS 异步任务
- setTimeout 和 setInterval
- DOM事件
- ES6中的 Promise


### 字符串转换成数字
	
	如： '123.23' --> +'123.23'

### toFixed保留整数,会将数字转换成字符串
	var a=123.36896335.toFixed(2)
	console.log(a)//'123.37' --> a 已经是字符串
	通过 + 号可以变成数字，不是整数，是原来的数字
	a = +a
	console.log(a)//123.37  --> a 又变成数字


### 函数节流

	function delayFn (fn, delay, mustDelay){
	     var timer = null;
	     var t_start;
	     return function(){
	         var context = this, args = arguments, t_cur = +new Date();
	         //先清理上一次的调用触发（上一次调用触发事件不执行）
	         clearTimeout(timer);

	         //如果不存触发时间，那么当前的时间就是触发时间
			 // 这个可以放到外层
	         if(!t_start){
	             t_start = t_cur;
	         }
	         //如果当前时间-触发时间大于最大的间隔时间（mustDelay），触发一次函数运行函数
	         if(t_cur - t_start >= mustDelay){
	             fn.apply(context, args);
	             t_start = t_cur;
	         }
	         //否则延迟执行
	         else {
	             timer = setTimeout(function(){
	                 fn.apply(context, args);
	             }, delay);
	         }
	     };
	}

	// 测试用例：
	var count=0;
	function fn1(){
	    count++;
	    console.log(count)
	} 
	//100ms内连续触发的调用，后一个调用会把前一个调用的等待处理掉，但每隔200ms至少执行一次
	document.onmousemove=delayFn(fn1,100,200)



### js中的innerHTML，innerText，value的区别

	innerHTML  是在控件中加html代码  就是设置一个元素里面的HTML
	document.getElementById("demo").innerHTML="<h1>My First JavaScript</h1>"; 

	innerText   在控件中添加文字，W3C没有标准化，尽量用innerHTML
	document.getElementById("demo").innerText="<h2>My First JavaScript</h2>";  
	h2标签不会被解析，会当作字符串显示。

	value 是专门用作表单的，读取或者写入表单的数据
	document.getElementById("input").value="测试数据";  








