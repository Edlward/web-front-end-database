
## 使用jQuery下载网页上的图片

	function downloadImage(src) {
	    var a = $("<a></a>").attr("href", src).attr("download", "img.png").appendTo("body");
	
	    a[0].click();
	    a.remove();
	}











