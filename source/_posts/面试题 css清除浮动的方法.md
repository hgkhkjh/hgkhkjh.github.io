---
title: 面试题 css清除浮动的方法
date: 2022-04-7 16:43:45
tags:
---
## 浮动的产生

> 1、普通流定位 static（默认方式）
普通流定位，又称为文档流定位，是页面元素的默认定位方式
页面中的块级元素：按照从上到下的方式逐个排列
页面中的行内元素：按照从左到右的方式逐个排列
但是如何让多个块级元素在一行内显示?
这里就引出了浮动定位


>2、浮动定位 float
float属性 取值为 left/right
这个属性原本不是用来布局的，而是用来做文字环绕的，但是后来人们发现做布局也不错，就一直这么用了，甚至有些时候都忘了用他做文字环绕


>3、相对定位 relative
元素会相对于它原来的位置偏移某个距离，改变元素位置后，元素原本的空间依然会被保留
语法
属性：position
取值：relative
配合着 偏移属性(top/right/bottom/left)实现位置的改变


>4、绝对定位 absolute
如果元素被设置为绝对定位的话，将具备以下几个特征
1、脱离文档流-不占据页面空间
2、通过偏移属性固定元素位置
3、相对于 最近的已定位的祖先元素实现位置固定
4、如果没有已定位祖先元素，那么就相对于最初的包含块(body,html)去实现位置的固定
语法
属性：position
取值：absolute
配合着 偏移属性(top/right/bottom/left)实现位置的固定


>5、固定定位 fixed
将元素固定在页面的某个位置处，不会随着滚动条而发生位置移动
语法
属性：position
取值：fixed
配合着 偏移属性(top/right/bottom/left)实现位置的固定


## 第一种 clear:both

> 取值：
1、none
默认值，不做任何清除浮动的操作
2、left
清除前面元素左浮动带来的影响
3、right
清除前面元素右浮动带来的影响
4、both
清除前面元素所有浮动带来的影响
优势：代码量少 容易掌握 简单易懂
本声明。


设置浮动
```javascript
<!DOCTYPE html>
<html lang="en">

<head>
	<meta charset="UTF-8">
	<meta http-equiv="X-UA-Compatible" content="IE=edge">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>Document</title>
</head>
<style>
	div{
		width: 300px;
		height: 300px;
		border: 2px solid white;
		background-color: pink;
		float: left;
	}
</style>
<body>
<div></div>
<div></div>
<div></div>
<div></div>
<div></div>
<div></div>
<div></div>
</body>
</html>
<script>
</script>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/c9b4b0bb39964fa4bd78ea5872b96641.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5oiR6KaB5a2m5YmN56uv44CC,size_20,color_FFFFFF,t_70,g_se,x_16)
设置clear:both清除浮动
![在这里插入图片描述](https://img-blog.csdnimg.cn/cf7a25488d8f40ed8f03ba6b33c4ec6e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5oiR6KaB5a2m5YmN56uv44CC,size_20,color_FFFFFF,t_70,g_se,x_16)

> clear：both：本质就是闭合浮动， 就是让父盒子闭合出口和入口，不让子盒子出来

## 第二种 额外标签法

> 额外标签法（在最后一个浮动标签后，新加一个标签，给其设置clear：both；）

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
	<meta charset="UTF-8">
	<meta http-equiv="X-UA-Compatible" content="IE=edge">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>Document</title>
</head>
<style>
	.div{
		width: 300px;
		height: 300px;
		border: 2px solid white;
		background-color: pink;
		float: left;
	}
   .father{
	   border: 1px solid steelblue;
   }
</style>

<body>
	<div class="father">
		<div class="div"></div>
		<div  class="div"></div>
		<div  class="div"></div>
		<div  class="div"></div>
		<div  class="div"></div>
		<div  class="div"></div>
		<div style="clear: both;"></div>
	</div>
</body>

</html>
<script>
</script>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/5f295aead7b74dc3bdc45ff60938356b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5oiR6KaB5a2m5YmN56uv44CC,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/7e887d193f534ac1bb18be2c4f1c6a3c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5oiR6KaB5a2m5YmN56uv44CC,size_20,color_FFFFFF,t_70,g_se,x_16)
## 第三种父元素添加overflow:hidden 或 auto

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
	<meta charset="UTF-8">
	<meta http-equiv="X-UA-Compatible" content="IE=edge">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>Document</title>
</head>
<style>
	.div{
		width: 300px;
		height: 300px;
		border: 2px solid white;
		background-color: pink;
		float: left;
	}
   .father{
	   border: 1px solid steelblue;
	   overflow:hidden
   }
</style>

<body>
	<div class="father">
		<div class="div"></div>
		<div  class="div"></div>
		<div  class="div"></div>
		<div  class="div"></div>
		<div  class="div"></div>
		<div  class="div"></div>
		<div  class="div"></div>
	</div>
</body>

</html>
<script>
</script>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2064514d1df64a31bb9ae793d46863f7.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5oiR6KaB5a2m5YmN56uv44CC,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/967982e8c17a496ab1773e0ca8045bae.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5oiR6KaB5a2m5YmN56uv44CC,size_20,color_FFFFFF,t_70,g_se,x_16)
## 第四种 父元素添加：after伪类清除浮动

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
	<meta charset="UTF-8">
	<meta http-equiv="X-UA-Compatible" content="IE=edge">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>Document</title>
</head>
<style>
	.div {
		width: 300px;
		height: 300px;
		border: 2px solid white;
		background-color: pink;
		float: left;
	}

	.father {
		border: 1px solid steelblue;
	}

	.father:after {
		content: "";
		display: block;
		clear: both;
	}

</style>

<body>
	<div class="father">
		<div class="div"></div>
		<div class="div"></div>
		<div class="div"></div>
		<div class="div"></div>
		<div class="div"></div>
		<div class="div"></div>
		<div class="div"></div>
	</div>
</body>

</html>
<script>
</script>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/929c94a4078f46baa1f8a22ed385e88f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5oiR6KaB5a2m5YmN56uv44CC,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/1db26386f30041638c5c731502f7b7eb.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5oiR6KaB5a2m5YmN56uv44CC,size_20,color_FFFFFF,t_70,g_se,x_16)
## 第五种父元素添加display: table;

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
	<meta charset="UTF-8">
	<meta http-equiv="X-UA-Compatible" content="IE=edge">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>Document</title>
</head>
<style>
	.div {
		width: 300px;
		height: 300px;
		border: 2px solid white;
		background-color: pink;
		float: left;
	}

	.father {
		border: 1px solid steelblue;
		display: table;
	}
</style>

<body>
	<div class="father">
		<div class="div"></div>
		<div class="div"></div>
		<div class="div"></div>
		<div class="div"></div>
		<div class="div"></div>
		<div class="div"></div>
		<div class="div"></div>
	</div>
</body>

</html>
<script>
</script>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/5eb3f3474ae44fcba48654856fb6779d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5oiR6KaB5a2m5YmN56uv44CC,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/bff9fe1b495d44c88874967cc54a4379.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5oiR6KaB5a2m5YmN56uv44CC,size_20,color_FFFFFF,t_70,g_se,x_16)

