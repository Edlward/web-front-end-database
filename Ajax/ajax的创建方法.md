

## 创建一个原生的ajax
	GET请求：
	var xhr = new XMLHttpRequest();
	    // 按这个顺序写，避免浏览器兼容性问题
	    xhr.onreadystatechange = handler;
	    xhr.open('GET', "/a.php");
	    xhr.responseType = "json";
	
	    xhr.send(null);
	    function handler() {
	        if (xhr.readystate !== 4) {
	            return;
	        }
	        if (xhr.status === 200) {
	            console.log("接收到的数据 "+xhr.responseText);
	        } else {
	            new Error("错误状态码 "+xhr.status);
	        }
	    }
			
		POST请求：
		var xhr = new XMLHttpRequest();
	    // 按这个顺序写，避免浏览器兼容性问题
	    xhr.onreadystatechange = handler;
	    xhr.open('POST', "/a.php");
		// 设置头部信息模仿表单提交
	    xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
		// 获取表单的数据，然后提交表单数据
		// 根据需要自己获取表单的数据，这里空着没写
	    xhr.send("表单的数据，可以对数据先进行处理");
	    function handler() {
	        if (xhr.readystate !== 4) {
	            return;
	        }
	        if (xhr.status === 200) {
	            console.log("接收到的数据 "+xhr.responseText);
	        } else {
	            new Error("错误状态码 "+xhr.status);
	        }
	    }

## 用Promise对象实现的Ajax操作的例子


	var getJSON = function(url) {

		var promise = new Promise(function(resolve, reject){
			var client = new XMLHttpRequest();
			
			client.onreadystatechange = handler;
			client.responseType = "json";
			client.open("GET", url);
			client.setRequestHeader("Accept", "application/json");
			client.send(null);
	
			function handler() {
				if (this.readyState !== 4) {
					return;
				} 
				if (this.status === 200) {
					resolve(this.responseText);
				} else {
					reject(new Error(this.status));
				}
			};

		});

		return promise;
	};
	
	// 实例调用
	getJSON("/posts.json").then(function(json) {
		console.log('Contents: ' + json);
	}, function(error) {
		console.error('出错了', error);
	});



