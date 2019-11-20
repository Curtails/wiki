---
layout: page
title: "JavaScript学习"
data: 2019-11-20 10:30
---

[TOC]

# JavaScript简介 
[菜鸟教程 js](https://www.runoob.com/js/js-tutorial.html)  

[W3school js](https://www.w3school.com.cn/js/index.asp)
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

# JavaScript输出
**四种**方式输出数据  

## windows.alert()
windows.alert()同alert()
```html
<script>
	window.alert(5 + 6);
</script>
```

## 操作HTML元素
使用document.getElementById(id)访问某个元素
```html
<body>
<h1>我的第一个 Web 页面</h1>
<p id="demo">我的第一个段落。</p>
<script>
	document.getElementById("demo").innerHTML="段落已修改。";
</script>
</body>
```
## 写到 HTML 文档
使用document.write()向文档输出内容
```html
<body>
<h1>我的第一个 Web 页面</h1>
<p>我的第一个段落。</p>
<script>
	document.write(Date());
</script>
</body>
```
每次调用document.write()之前会调用open() 写完之后该函数会关闭  当再次调用该函数时就会刷新页面

## 写到控制台
使用console.log()在浏览器控制台显示内容
```html
<script>
	a = 5;
	b = 6;
	c = a + b;
	console.log(c);
</script>
```
打开控制台查看11

# JavaScript语法
## JavaScript字面量
字面量是值  

- 数字
- 字符串
- 表达式
- 数组
- 对象
- 函数

## JavaScript变量
var定义变量
```js
var x,length;
length = 666;
```
## JavaScript细节

- 字母大小写敏感
- JavaScript 使用 Unicode 字符集

# JavaScript数据类型

- 值类型(基本类型)：字符串（String）、数字(Number)、布尔(Boolean)、对空（Null）、未定义（Undefined）、Symbol。  
- 引用数据类型：对象(Object)、数组(Array)、函数(Function)。
## JavaScript 拥有动态类型
```js
var x;               // x 为 undefined
var x = 5;           // 现在 x 为数字
var x = "John";      // 现在 x 为字符串
```

## 字符串
```js
var answer="It's alright";
var answer="He is called 'Johnny'";
var answer='He is called "Johnny"';
```

## 数字
```js
var x1=34.00;      //使用小数点来写
var x2=34;         //不使用小数点来写
var y=123e5;      // 12300000
var z=123e-5;     // 0.00123
```

## 布尔
```js
var x=true;
var y=false;
```

## 数组
```js
var cars=new Array();
cars[0]="Saab";
cars[1]="Volvo";
cars[2]="BMW";
//或者condensed array
var cars=new Array("Saab","Volvo","BMW");
//再或者
var cars=["Saab","Volvo","BMW"];
```

## 对象
```js
var person={firstname:"John", lastname:"Doe", id:5566};
var person={
firstname : "John",
lastname  : "Doe",
id        :  5566
};

//两种寻址方式
name=person.lastname;
name=person["lastname"];
```
## Undefined 和 Null

- Undefined表示未定义
- Null表示值为空

## 声明变量
```js
var carname=new String;
var x=      new Number;
var y=      new Boolean;
var cars=   new Array;
var person= new Object;
```
声明变量要不直接赋值，要不就先new在赋值；函数也大致相同
```js
function Demo(){
    var obj=new Object();
    obj.name="张思";
    obj.age=12;
    obj.firstF=function(){
    }
    obj.secondF=function(){
    }
    return obj;
}
var one=Demo();
// 调用输出
document.write(one.age);

var one=new Demo;
// 调用输出
document.write(one.age);
```

## 判断数据类型格式
```js
var cars=new Array();
cars[0]="Saab";
cars[1]="Volvo";
cars[2]="BMW";
document.write(typeof cars); // 数组和对象typeof都是object
```

## 数据类型的内存分配
### 基本类型存放到栈
```js
var a,b;
a = "zyj";
b = a;
console.log(a);   // zyj
console.log(b);   // zyj
a = "呵呵";       // 改变 a 的值，并不影响 b 的值
console.log(a);   // 呵呵
console.log(b);   // zyj
```

### 引用类型存放到堆
```js
var a = {name:"percy"};
var b;
b = a;
a.name = "zyj";
console.log(b.name);    // zyj
b.age = 22;
console.log(a.age);     // 22
var c = {
  name: "zyj",
  age: 22
};
```

## 数据类型的简单转换

- 利用toString把数值转换成字符串

```html
<script>
	var a=100;
	var c=a.toString();
	alert(typeof(c));      //typeof()方法验证转换后的数据类型
</script>
```

- 使用 parseInt() 和 parseFloat() 方法可以把字符串转换为数值

```html
<script>
	var str="123.30";
	var a=parseInt(str);    
	//parseInt()方法把字符串转换为整数，parseFloat()方法把字符串转换为浮点数
	var b=parseFloat(str);
</script>
```

# JavaScript对象
- 在JavaScript中 对象是变量的容器
- 对象可包含变量和函数
## 对象定义
```js
var person = {firstName:"John", lastName:"Doe", age:50, eyeColor:"blue"};
var person = {
    firstName:"John",
    lastName:"Doe",
    age:50,
    eyeColor:"blue"
};
//前两者定义方式相同
//另一种定义对象方式
var person=new Object();
person.name='小明'；
person.sex='男'；
person.method=function(){
  	return this.name+this.sex;
}
```
## 对象属性
```js
person["firstName"];
person.firstName;
```

## 对象方法
```js
name = person.fullName();//访问了person对象的fullName方法
name = person.fullName； //输出person对象的fullName函数字符串
```

## 一些细节
- 对象由两个重复属性，以最后赋值的属性为准
- 对象是键值对的容器，键必须为字符串，值可以为null和undefined以外的任意数据类型

# JavaScript函数