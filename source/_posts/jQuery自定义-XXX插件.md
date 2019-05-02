title: jQuery自定义$.XXX插件
author: 学亮
date: 2019-04-28 15:52:10
tags:
---
```
<script src="js/jquery-1.8.3.js" type="text/javascript" charset="utf-8"></script>
	<script type="text/javascript">
		$.extend({
			maxValue:function(a,b){
				return a>b?a:b;
			},
			minValue:function(a,b){
				return a>b?b:a;
			}
		});
		
		$(function(){
			alert($.minValue(1,2))
		})
	</script>
```
