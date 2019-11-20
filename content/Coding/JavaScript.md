---
layout: page
title: "javascript学习"
data: 2019-11-20 10:30
---

[菜鸟教程 js](https://www.runoob.com/js/js-tutorial.html)

# JavaScript简介
它能干什么？
## 直接写入HTML输出流
```html
<script>
document.write("<h1>这是一个标题</h1>");
document.write("<p>这是一个段落。</p>");
</script>
```

## 对事件反应
```html
<button type="button2" onclick="alert('不欢迎!')">不点我！</button>
```

## 改变HTML内容
document.getElementById("demo") 利用了HTML的DOM  
```html
<script>
function myFunction()
{
	document.getElementById("demo").innerHTML="HELLO!";
}
</script>

<button type="button" onclick="myFunction()">点击这里</button>
```

## 改变HTML图像
实际上是JavaScript改变了img的属性src  
src.match("bulbon")意思为查找src是否包含"bulbon"字符串  
```html
<script>
function changeImage()
{
	element=document.getElementById('myimage')
	if (element.src.match("bulbon"))
	{
		element.src="/images/pic_bulboff.gif";
	}
	else
	{
		element.src="/images/pic_bulbon.gif";
	}
}
</script>
<img id="myimage" onclick="changeImage()" src="/images/pic_bulboff.gif" width="100" height="180">
<p>点击灯泡就可以打开或关闭这盏灯</p>
```

## 改变HTML样式
本质上也是改变HTML属性  
```html
<script>
    function myFunction()
	{
		x=document.getElementById("demo") // 找到元素
		x.style.color="#ff0000";          // 改变样式
    }
</script>
    <button type="button" onclick="myFunction()">点击这里</button>
    
//或者更高级一点
<script>
function changeImage(s){
    s.src = s.src.match('bulboff')?"/images/pic_bulbon.gif":"/images/pic_bulboff.gif";
}
</script>
<img id="myimage" onclick="changeImage(this)" src="/images/pic_bulboff.gif" width="100" height="180">
<p>点击灯泡就可以打开或关闭这盏灯</p>
```

## 验证输入
isNaN(x)判断是否为非数字值 
```html
<p>请输入数字。如果输入值不是数字，浏览器会弹出提示框。</p>
<input id="demo" type="text">
<script>
function myFunction()
{
	var x=document.getElementById("demo").value;
	if(x==""||isNaN(x))
	{
		alert("不是数字");
	}
}
</script>
<button type="button" onclick="myFunction()">点击这里</button>
```

# JavaScript用法

## <head>
```html
<!DOCTYPE html>
<html>
<head>
<script>
function myFunction()
{
    document.getElementById("demo").innerHTML="我的第一个 JavaScript 函数";
}
</script>
</head>
<body>
<h1>我的 Web 页面</h1>
<p id="demo">一个段落</p>
<button type="button" onclick="myFunction()">尝试一下</button>
</body>
</html>
```
## <body>
```html
<!DOCTYPE html>
<html>
<body>
<h1>我的 Web 页面</h1>
<p id="demo">一个段落</p>
<button type="button" onclick="myFunction()">尝试一下</button>
<script>
function myFunction()
{
    document.getElementById("demo").innerHTML="我的第一个 JavaScript 函数";
}
</script>
</body>
</html>
```

## 外部引用
注意js文件不要含有`<script>`标签
```html
<!DOCTYPE html>
<html>
<body>
<script src="myScript.js"></script>
</body>
</html>
```

