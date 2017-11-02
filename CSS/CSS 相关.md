## 如果设置为表格布局则margin无效
	display: table;
	这时可以用 
	border-collapse;
	border-spacing: 5px;
	让表格有间隔

## 为表格设置合并边框模型
	table
	  {
	  	border-collapse:collapse;
	  }
	
	table, td, th
	  {
	  	border:1px solid black;
	  }


## 表格布局的优缺点

	缺点:
	1、 标签结构多，复杂 
	我们可以从表格的结构来看
	 <table>  <tr>  
	 <td>内容1</td> </tr> 
	</table> 
	从上面表格的结构标签来看，标签的对数较多，在表格布局中，主要是用到表格的相互嵌套使用，这样就会造成代码的复杂度更高！ 
	2、 表格布局，不利于搜索引擎抓取信息，直接影响到网站的排名
	
	优点:
	1、结构位置更简单 
	2、容易上手 
	3、 数据化的存放更合理。
	例如，学生信息这样的表，如果要在网页中显示，表格布局反而会更简单，相反利用div+CSS会使得事情更加复杂化！


## 如何取得元素的宽和高
	-dom.style.width/height 
		dom 如 document.getElementById("box");
		但是这样只能取得内联样式的宽和高
		CSS加载有3种方法： 
			1.直接在html中写入style--为行内/内联样式
			2.在html的head中用style标签写css--内部样式
			3.通过link加载的外部样式


	下面方法可以获得外部或者内部样式的宽或者高
        
		// currentStyle 只有IE支持
		// console.log('ie' ,divDom[0].currentStyle.width);

		// firefox 支持
		// console.log('firefox', divDom[0].getComputedStyle.width);
        
		// firefox 和 chrome 支持 有单位 px， 如 300px
		// console.log( window.getComputedStyle(divDom[0]).width);
       
		 // ie / firefox 和 chrome 支持
        console.log(divDom[0].getBoundingClientRect().width);



### 相对浏览器定位

	position: fixed; 相对浏览器定位的直接父元素一定要是相对定位： position: relative;
	而且相对浏览器定位要设置 z-index: 10 这个值，值越大在越上面，如果点击不到元素就是补其他的元素
	遮盖了，设置在一引起的 z-index 值就可以。
	弹窗一般放在浏览器的最后面，但是有时候也会点击不到，这个时候可以把所有的弹窗都放到浏览器底部如：
	/*  所有弹窗的位置都在浏览器最底部*/
	.dialog-position {
	    position: fixed;
	    bottom: -10px;
	}
	也可以设置弹窗中 position的位置：
	弹窗最外层样式：
	 .dialog-wrap {
        /*position: fixed;*/
        position: relative;
        width: 100%;
        height: 100%;
    }


### a 标签中加入 img 图片外部点击离开时会有外边框

	这个时候设置 a 的 outline: none 即可，同时也设置图片的 border: 0










