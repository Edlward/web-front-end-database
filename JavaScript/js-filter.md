
## filter

- filter是Javascript中Array常用的操作，它用于把Array的某些元素过滤掉，然后返回剩下的元素，数组自身是没有改变的。

- filter把传入的函数依次作用于每个元素，然后根据返回值是 true 还是false决定保留还是丢弃该元素。

		如：
		let arrx = [1, 2, 3, 1, 3, 5];
	    // 返回值大于2的元素
	    let rex = arrx.filter( x => {
	        return x > 2;
	    } );
	    console.log('rex:', rex);
	    console.log('arrx:', arrx);  // 数组自身没有改变
	
		如：
		let arrx = [1, 2, 3, 1, 3, 5];
	    // 过滤自身重复元素
	    let rej_2 = arrx.filter( (value, index, arr) => {
	        return arr.indexOf(value) === index ;
	    } );
	    console.log('rej_2:', rej_2);
	    console.log('arrx:', arrx);

		如果支持ES6数组去重有更简洁的方法：
		[...new Set(arrx)]

