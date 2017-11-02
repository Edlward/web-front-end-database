
## DOCTYPE 是什么

	DOCTYPE标签是文档类型声明，它的目的是要告诉标准通用标记语言解析器，它应该使用什么样的文档类型定义（DTD）来解析文档。

	<!DOCTYPE> 声明必须是 HTML 文档的第一行，位于 <html> 标签之前。

	<!DOCTYPE> 声明不是 HTML 标签；它告诉 浏览器，页面是使用哪个版本的 HTML 来进行编写的，然后浏览器就会用对应文档类型定义（DTD）来解析	HTML代码。

	在 HTML 4.01 中，<!DOCTYPE> 声明引用 DTD，因为 HTML 4.01 基于 SGML。DTD 规定了标记语言的规则，这样浏览器才能正确地呈现内容。
	HTML5 不基于 SGML，所以不需要引用 DTD。

**提示：请始终向 HTML 文档添加 <!DOCTYPE> 声明，这样浏览器才能获知文档类型。**

### DOCTYPE类型

- 在 HTML 4.01 中有三种 <!DOCTYPE> 声明。

		HTML 4.01 Strict--严格格式
		该 DTD 包含所有 HTML 元素和属性，但不包括展示性的和弃用的元素（比如 font）。不允许框架集（Framesets）。
		<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">

		HTML 4.01 Transitional
		该 DTD 包含所有 HTML 元素和属性，包括展示性的和弃用的元素（比如 font）。不允许框架集（Framesets）。
		<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" 
		"http://www.w3.org/TR/html4/loose.dtd">

		HTML 4.01 Frameset
		该 DTD 等同于 HTML 4.01 Transitional，但允许框架集内容。
		<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Frameset//EN" 
		"http://www.w3.org/TR/html4/frameset.dtd">

- 在 HTML5 中只有一种：
	<!DOCTYPE html>



### textArea中的placeholder属性不起作用

	<textarea>与</textarea>之间有间隔，或存在换行；
	这样写一行就会出现placeholder的内容
	<textarea placeholder="请输入聊天内容" class="chat"></textarea>


### 表单写法

	<div>
	    <form action="/regist" method="post">
	        <div class="formField">
	            <label for="username">用户名： </label> <input type="text" class="item" name="username" id="username">
	        </div>
	        <div class="formField">
	            <label for="password">密码： </label> <input type="password" class="item" name="password" id="password">
	        </div>
	        <div class="formField">
	            <label for="user-password">确认密码： </label> <input type="password" class="item" name="user-password" id="user-password">
	        </div>
	        <div class="formField">
	            <label for="phone">手机号： </label> <input type="number" class="item" name="phone" id="phone">
	        </div>
	        <button type="submit">注册</button>
	    </form>
	</div>

	样式：
	
	form .formField {
	    overflow: auto;
	    width: 100%;
	    margin: 5px 0;
	}
	form label {
	    float: left;
	    display: block;
	    margin-right: 10px;
	    /*text-transform: capitalize;*/
	    text-align: right;
	    min-width: 100px;
	}
	input.item {
	    display: block;
	    float: left;
	}


	提交表单会刷新页面！！！


### HTML5 中的 localstorage

	localstorage 在浏览器关闭时，数据不会丢失。

- 存储值

		localstroage.setItem('store-key','要存储的数据格式要是字符串');  
		如：
		要存储的数据的key值
		const STORAGE_KEY = 'todo-v1.0-key';
		save (todos) {
	            localStorage.setItem(STORAGE_KEY, JSON.stringify(todos))
	     }

- 获取值

		var openid = localstroage.getItem('store-key');  
		如：
	        fetch () {
	            var todos = JSON.parse(localStorage.getItem(STORAGE_KEY)
	                || '[{"title":"请输入您的todo任务", "completed":false}]');
	            
				//            todos.forEach((todo, index) => { todo.id = index });
				//            todoStorage.uid = todos.length;
				            return todos;
			},


- 删除某个值

		localstroage.removeItem('store-key');  


- 删除所有值

		localstroage.clear();



### 响应式布局

	在响应式布局中，之前写的样式，如果在后来写的样式中没有重写，则会继承前面的样式，所以有些样式要重写如：
	        position: fixed;
            top: auto;
            right: auto;
            bottom: 0;
		写auto恢复默认值，写0的话就会响应现在的布局。



### 给DIV增加 可以输入属性

	contenteditable="true" 设置这样就可以输入数据了。
	如：
    <div :class="['waitEdit', {'edit': todo == editedTodo}]"
     contenteditable="true"
     v-focus="todo == editedTodo"
     ref="edit"
     v-html="todo.title"
     @blur="doneEdit(todo)"
     @keyup.esc="cancelEdit(todo)"
	>
	    <!--{{ todo.title }}-->
	</div>

	参考： http://www.zhangxinxu.com/wordpress/2010/12/div-textarea-height-auto/









