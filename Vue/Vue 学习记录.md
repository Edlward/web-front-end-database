
## 安装 vue-cli
	
	npm i vue-cli -g
	使用：
		vue init webpack-simple my-project
		npm install 
		npm run dev

	vue list 可以查看 vue-cli 提供的项目模板

## vue 中的命令

	<a :link="url">taobao</a>
	<a link="url">taobao</a>

	上面的是由v-blind命令绑定的，下面是原生的link,所以上面url是一个变量定义在vue data中，
	下面的是字符串这个url要改成真正的url,如：http://www.taobao.com
	其他的命令也一样，如：
	<p v-html="msg"></p> 中的 msg 也是变量

## computed 计算属性
	
	computed 里的数据和 data 里的数据是一样的，只是computed里的数据会根据内部的值而更新自己的值，如
	computed:{
		computedValue () {
			return this.dataValue + 10;
		}
	}
	dataValue 为 data里定义的数据
	computed 是 根据 dataValue的变化重新计算 computedValue的值，这个和watch 监听正好相反。

## watch 监听事件
	
		watch: {
            watchValue: function (val, oldVal) {
	            console.log(val, oldVal);
            }，

			example2:{
　　　　　　　　　//注意：当观察的数据为对象或数组时，curVal和oldVal是相等的，因为这两个形参指向的是同一个数据对象
　　　　　　　　　　handler(curVal,oldVal){
　　　　　　　　　　　　conosle.log(curVal,oldVal)
　　　　　　　　　　},
　　　　　　　　　　deep:true
			　　　}
		}
		这个是监听watchValue的变化，如果这个值变化了，就运行右边的函数，输出当前的值和变化之前的值，这个函数接收的两个参数是默认转递过来的。
	


## v-if 和 v-show 区别

	v-if如果条件为false时，会将当前DOM节点删除，而v-show不会删除，只会将display设置为none,让其隐藏。

	<p v-show="isShow">  show </p>

 	<p v-if="isShow1"> show1 </p>


## 组件 生命周期
	之前 1.xx:
		init	
		created
		beforeCompile
		compiled
		ready		√	->     mounted
		beforeDestroy	
		destroyed

	现在 2.xx:
		beforeCreate	组件实例刚刚被创建,属性都没有
		created	实例已经创建完成，属性已经绑定
		beforeMount	模板编译之前
		mounted	模板编译之后，代替之前ready  *
		beforeUpdate	组件更新之前
		updated	组件更新完毕	*
		beforeDestroy	组件销毁前
		destroyed	组件销毁后



### 组件间通信
	
		A组件发出信号：

		import { eventBus } from '../../eventBus'

            // 点击最外部组件 处理程序
            resetSelect() {
                // 发送全局事件，监听有这个事件的组件会作出相应的反应
                eventBus.$emit('reset-component');
            }


		B组件监听：

		import { eventBus } from '../../eventBus'

        mounted() {
            eventBus.$on('reset-component', () => {
                this.isDrop = false;
            })
        },

		监听到A发出事件时，就作出对应处理，其中eventBus.js数据就是一个全局的vue：

		import Vue from 'vue'
		// 用作--发送--公共事件
		const eventBus = new Vue();
		
		export {eventBus }


### 父子组件通信

	子组件接收数用props
        props: {
            isShow: {
                type: Boolean,
                default: false
            }
        },
	父组件传递方法： 是用属性邦定：
	
	    <my-dialog :is-show="isShowLogDialog" @on-close="closeDialog('isShowLogDialog')">
            <log-form @has-log="onSuccessLog"></log-form>
        </my-dialog>

	同时父组件监听子组件的事件 on-close 当子组件
	       closeMyself () {
                this.$emit('on-close', "传递给父组件的数据也可以没有")
            }
	发送事件时，父组件就会接收到，并且作出对应的处理，同时子组件也可以传递数据。


### 路由跳转

- 软件自动跳转

		toOrderList () {
		    this.$router.push({path: '/orderList'});
		}

- 点击跳转

		<ul>
			<router-link v-for="(item, index) in products"
			    :to="{ path: item.path }"
			    tag="li" active-class="active"
			    :key="index"
			>
			    {{ item.name }}
			</router-link>
		</ul>


### 路由跳转回到页面顶部	

	在router/index.js 文件下修改Router内部即可：

	export default new Router({
    mode: 'history',
    routes: [
        {
            path: '/',
            name: 'HomePage',
            component: HomePage
        },
        {
            path: '/product/:id',
            component: ProductDetail
        }

    ],
    // 让路由跳转到顶部
    scrollBehavior (to, from, savedPosition) {
        return { x: 0, y: 0 }
    }
})


### Vuex 

	Vuex中接收外部参数的方法
	// 可以异步操作
	const actions = {
	    fetchProductList ({commit}) {
	        commit('updateProductList', products);
	    },
	    // 获取对应ID的产品，ID为外部传递过来
	    getProductOfId ({commit}, id) {
	        commit('updateProductList', products);
	        commit('updateProductOfId', id);
	    }
	
	};

	调用：

	created(){
			// vuex 中传递参数方法
            this.$store.dispatch('getProductOfId',
	            parseInt(this.$route.params.id));
        },


### You may have an infinite update loop in a component render function
	
	在vue中报这个错误可能是 
	
		- 直接在 template 里操作vue里的data 变量，可以把操作变量放到函数中
		- 在v-for中对data里的变量进行++操作，可以把data里的变量放到export default外面进行声明



### 失去焦点事件

	@blur="checkTitle"



### webpack跨域设置

- 用webpack-simple模板的vue项目设置方法
	
		在webpack.config.js中进行配置即可。
	
		在module.exports 中配置
		devServer: {
		      historyApiFallback: true,
		      noInfo: true,
		
		    // 设置跨域相关
		    proxy: {
		      // 请求到 '/api' 下 的请求都会被代理到 target：http://localhost:8002 中
		      '/api': {
		          target: 'http://localhost:8002',
		          secure: false, // 接受 运行在 https 上的服务
		          changeOrigin: true
		      }
		    }
		  	},
		其他的配置都不变。

- 用webpack模板的vue项目设置方法

	这个网上有，可以参考，因为没用过webpack模板的vue进行跨域开发，所以没有配置过。




