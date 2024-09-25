#  HTML

## 起步

文档声明

```html
<!doctype html> <!--h5 声明-->
```

### 常见字符集

* asc ii
* Iso-8859-1
* gbk 中国编码
* gb2312 中国编码
* utf-8 万国码

```html
<!doctype html>
<html>
	<head>
		<!--
			meta自结束标签，用来设置网页的一些元数据如网页的字符集，关键字、简介
		-->
		<meta charset="utf-8" />
		<title>网页的标题</title>
	</head>
	<body>
		<h1>这是一个非常漂亮的网页</h1>
	</body>
</html>
```

## 基本标签

### 元数据

```html
<!doctype html>
<html>
	<head>
		<meta charset="utf-8" />
		<title></title>
		<!-- 
			使用meta标签还可以用来设置网页的关键字
		-->
		<meta name="keywords" content="HTML5,JavaScript,前端,Java" />
		<!-- 
			指定网页描述。搜索引擎会同时检索页面中的关键词和描述。
		-->
		<meta name="description" content="发布h5、js等前端相关的信息" />
		<!-- 
			使用meta设置请求的重定向
		-->
		<meta http-equiv="refresh" content="5;url=http://www.baidu.com" />
	</head>
	<body>
		<h1>5秒以后跳转页面</h1>
	</body>
</html>
```



