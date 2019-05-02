title: jQuery DOM删除操作
author: 学亮
tags:
  - jquery
categories: []
date: 2019-04-28 16:25:00
---
```
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>dom插入--jquery</title>

<script type="text/javascript" src="js/jquery-1.8.3.js"></script>

<script type="text/javascript">
	$(function(){
		//内部插入
		//$("#edu").prepend($("<option value='学前班'>学前班</option>"));
		//$("<option value='学前班'>学前班</option>").prependTo($("#edu")); 
		
		//在学术硕士后面增加一项"研究生"
		//$("#edu").append($("<option value='研究生'>研究生</option>"));
		//$("<option value='研究生'>研究生</option>").appendTo($("#edu"));
		
		//外部插入
		//$("#edu option:first").before($("<option value='学前班'>学前班</option>"));
		//$("#edu option:last").after($("<option value='研究生'>研究生</option>"));
		
		//给p注册事件
		$("#d1 p").click(function(){
			//var p = $("#d1 p").remove();
			//var p =$("#d1 p").detach();
			$("#d1").empty();
		})
		//$("#d2").append(p);
		
	})
	
	
	
</script>
</head>
<body>
	<select id="edu">
		<option value="小学">小学</option>
		<option value="中学">中学</option>
		<option value="本科">本科</option>
		<option value="大专">大专</option>
		<option value="学术硕士">学术硕士</option>
	</select>
	
	<div id="d1">
		<p>aaaaaaaaa</p>
		<span>sssssssss</span>
	</div>
	<div id="d2"></div>
</body>
</html>
```