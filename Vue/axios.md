

## axios 基本配置

	Vue.prototype.$http = axios  //其他页面在使用axios的时候直接  this.$http就可以了

	
	
	//axios的一些配置，比如发送请求显示loading，请求回来loading消失之类的
	
	axios.interceptors.request.use(function (config) {  //配置发送请求的信息
	  stores.dispatch('showLoading')  // 发送请求时，运行之前个函数里面的语句
	  return config;
	}, function (error) {
	  return Promise.reject(error);
	});
	
	axios.interceptors.response.use(function (response) { //配置请求回来的信息
	  stores.dispatch('hideLoading')  // 收到返回信息时，运行之前个函数里面的语句
	  return response;
	}, function (error) {
	
	  return Promise.reject(error);
	});











