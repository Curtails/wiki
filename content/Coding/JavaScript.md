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

## head标签
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
## body标签
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

# JavaScript字符串
## 字符串
```js
//基本操作
var carname = "Volvo XC60";
var carname = 'Volvo XC60';
var character = carname[7];
//字符串包含单/双引号
var answer = "It's alright";
var answer = "He is called 'Johnny'";
var answer = 'He is called "Johnny"';
//添加转义符号使用单/双引号
var x = 'It\'s alright';
var y = "He is called \"Johnny\"";
```

## 字符串长度
```js
var txt = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
var sln = txt.length;
```

## 字符串可以是对象
使用 new 关键字将字符串定义为一个对象：var firstName = new String("John")
```js
<p id="demo"></p>
<script>
var x = "John";              // x是一个字符串
var y = new String("John");  // y是一个对象
document.getElementById("demo").innerHTML =typeof x + " " + typeof y;
</script>
```
建议不要创建String对象 当不同字符串进行强类型比较时会返回false

## 字符串属性
```
constructor	    返回创建字符串属性的函数
length	        返回字符串的长度
prototype	    允许您向对象添加属性和方法
```

## 字符串方法
更多 [方法实例](https://www.runoob.com/jsref/jsref-obj-string.html)
```
charAt()	    返回指定索引位置的字符
charCodeAt()	返回指定索引位置字符的 Unicode 值
concat()	    连接两个或多个字符串，返回连接后的字符串
fromCharCode()	将 Unicode 转换为字符串
indexOf()	    返回字符串中检索指定字符第一次出现的位置
lastIndexOf()	返回字符串中检索指定字符最后一次出现的位置
localeCompare()	用本地特定的顺序来比较两个字符串
match()	        找到一个或多个正则表达式的匹配
replace()	    替换与正则表达式匹配的子串
search()	    检索与正则表达式相匹配的值
slice()	        提取字符串的片断，并在新的字符串中返回被提取的部分
split()	        把字符串分割为子字符串数组
substr()	    从起始索引号提取字符串中指定数目的字符
substring()	    提取字符串中两个指定的索引号之间的字符
toLocaleLowerCase()	根据主机的语言环境把字符串转换为小写，只有几种语言（如土耳其语）具有地方特有的大小写映射
toLocaleUpperCase()	根据主机的语言环境把字符串转换为大写，只有几种语言（如土耳其语）具有地方特有的大小写映射
toLowerCase()	把字符串转换为小写
toString()	    返回字符串对象值
toUpperCase()	把字符串转换为大写
trim()	        移除字符串首尾空白
valueOf()	    返回某个字符串对象的原始值
```
方法使用例子：
```js
var x = "JohnJohn";  // x 是字符串
y = x.charAt(2);    // h
y = x.charCodeAt(2); // 104
y = x.concat(y, y); // JohnJohn104104, x+y+y
y = x.indexOf('h'); // 2, 索引从0开始
y = x.lastIndexOf('h'); // 6
y = x.slice();
y = x.split('o'); //J,hnJ,hn
y = x.substr(2); // hnJohn
y = x.substring(2,4) // hn，[2,3]
y = x.toLocaleLowerCase(); // johnjohn,小写
y = x.toLocaleUpperCase(); // JOHNJOHN,大写
y = x.toString(); // 转成Stirng
y = x.toUpperCase(); // JOHNJOHN,大写
y = x.trim(); // JohnJohn,去除两端的空格
y = x.valueOf(); // 返回某个字符串对象的原始值
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
没啥可写 放个示例
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>菜鸟教程(runoob.com)</title>
</head>
<body>
    看书：
    <input type="checkbox" name="checkbox" value=1>
    <br>
    写字：
    <input type="checkbox" name="checkbox" value=2>
    <br>
    打飞机：
    <input type="checkbox" name="checkbox" value=3>
    <br>
    玩游戏：
    <input type="checkbox" name="checkbox" value=4>
    <br>
    <button onclick="allcheck()">全选/取消</button>
    <script>
    var checkAll = false;
    function allcheck() {
        let inputs = document.getElementsByName("checkbox");
        var checkInt = 0;
        for (var i = 0; i < inputs.length; i++) {
            if (inputs[i].checked) {
                checkInt += 1;
            }
        }
        if (checkInt == inputs.length) {
            checkAll = false;
            for (var i = 0; i < inputs.length; i++) {
                inputs[i].checked = checkAll
            }
        } else {
            checkAll = true;
            for (var i = 0; i < inputs.length; i++) {
                inputs[i].checked = checkAll
            }
        }
    }
    </script>
</body>
</html>
```

## 向未声明的 JavaScript 变量分配值
如果给一个尚未声明的变量赋值 该变量将自动作为windows的一个属性
```js
cname = "test";//将声明windows的一个属性cname
```
非严格模式下创建的未声明的全局变量 是**全局对象**的可配置属性可以删除
```js
var var1 = 1; // 不可配置全局属性
var2 = 2; // 没有使用 var 声明，可配置全局属性

console.log(this.var1); // 1
console.log(window.var1); // 1

delete var1; // false 无法删除
console.log(var1); //1

delete var2; 
console.log(delete var2); // true
console.log(var2); // 已经删除 报错变量未定义
```

# JavaScript作用域

## 局部作用域
```js
// 此处不能调用 carName 变量
function myFunction() {
    var carName = "Volvo";
    // 函数内可调用 carName 变量
}
```

## 全局变量
一般情况
```js
var carName = " Volvo";
// 此处可调用 carName 变量
function myFunction() {
    // 函数内可调用 carName 变量
}
```
未声明(var)的变量未全局变量
```js
// 此处可调用 carName 变量
function myFunction() {
    carName = "Volvo";
    // 此处可调用 carName 变量
}
```
## 关于let关键字
let 声明的变量只在其声明的块或子块中可用
区别如下：
```js
varTest();
letTest();
function varTest() {
  var x = 1;
  if (true) {
    var x = 2;  // 同样的变量!
    console.log(x);  // 2
  }
  console.log(x);  // 2
}
function letTest() {
  let x = 1;
  if (true) {
    let x = 2;  // 不同的变量
    console.log(x);  // 2
  }
  console.log(x);  // 1
}
```

# JavaScript事件
## HTML事件
HTML事件实例

- HTML 页面完成加载
- HTML input 字段改变时
- HTML 按钮被点击

```js
//修改其他元素内容
<body>
<button onclick="getElementById('demo').innerHTML=Date()">现在的时间是?</button>
<p id="demo"></p>
</body>
//修改自身内容
<body>
<button onclick="this.innerHTML=Date()">现在的时间是?</button>
</body>
//调用其他函数
<body>
<button onclick="displayDate()">点这里</button>
<script>
function displayDate(){
	document.getElementById("demo").innerHTML=Date();
}
</script>
<p id="demo"></p>
</body>
```

## 常见的HTML事件
```
onchange	    HTML 元素改变
onclick     	用户点击 HTML 元素
onmouseover	    用户在一个HTML元素上移动鼠标
onmouseout	    用户从一个HTML元素上移开鼠标
onkeydown	    用户按下键盘按键
onload	        浏览器已完成页面的加载
```
更多事件参考 [HTML DOM 事件](https://www.runoob.com/jsref/dom-obj-event.html)

## 一些细节
不推荐使用HTML元素中可以**添加事件属性**的方式来添加属性 例如：
```js
<button onclick="getElementById('demo').innerHTML=Date()">现在的时间是?</button>
```
可以采用添加**事件句柄**的形式
```html
<body>
<button id="test">现在的时间是?</button>
<script>
window.onload = function(){
    var test = document.getElementById("test");   
    test.addEventListener("click",myfun1);
	test.addEventListener("click",myfun3);
}
function myfun1(){  
    alert("你好1");
}
function myfun3(){
	document.write(Date());	
}
</script>
</body>
```
编程原则要遵从**高内聚 低耦合** 或者一个形象比拟：**严于律己，宽以待人**

# 运算符
## +运算符
```js
txt1="What a very";
txt2="nice day";
txt3=txt1+txt2;
```
## 对字符串和数字进行加法运算
字符串和数字，数字转换为字符串

# JavaScript比较
- 弱比较
- 强比较
- 逻辑运算符
- 条件运算符

# JavaScript循环语句
## 循环
for-in循环
```js
var person={fname:"Bill",lname:"Gates",age:56}; 
for (x in person){
	txt=txt + person[x];
}
```
一些有特点的循环形式
```js
cars=["BMW","Volvo","Saab","Ford"];
var i=0;
for (;cars[i];){
	document.write(cars[i] + "<br>");
	i++;
}

cars=["BMW","Volvo","Saab","Ford"];
var i=0;
while (cars[i]){
	document.write(cars[i] + "<br>");
	i++;
}
```

## continue和break
跳出代码块
```js
cars=["BMW","Volvo","Saab","Ford"];
list:{
	document.write(cars[0] + "<br>"); 
	document.write(cars[1] + "<br>"); 
	document.write(cars[2] + "<br>"); 
	break list;
	document.write(cars[3] + "<br>"); 
	document.write(cars[4] + "<br>"); 
	document.write(cars[5] + "<br>"); 
}
```
# JavaScript typeof
 typeof 操作符来检测变量的数据类型
```js
typeof "John"                // 返回 string
typeof 3.14                  // 返回 number
typeof false                 // 返回 boolean
typeof [1,2,3,4]             // 返回 object
typeof {name:'John', age:34} // 返回 object
```

```js
typeof undefined             // undefined
typeof null                  // object
null === undefined           // false
null == undefined            // true
```

# JavaScript 类型转换
5种数据类型

- String
- number
- boolean
- object
- function

三种对象类型

- Object
- Date
- Array

不包含任何值的数据类型

- null
- undefined

## typeof 操作符
```js
typeof "John"                 // 返回 string
typeof 3.14                   // 返回 number
typeof NaN                    // 返回 number
typeof false                  // 返回 boolean
typeof [1,2,3,4]              // 返回 object
typeof {name:'John', age:34}  // 返回 object
typeof new Date()             // 返回 object
typeof function () {}         // 返回 function
typeof myCar                  // 返回 undefined (如果 myCar 没有声明)
typeof null                   // 返回 object
```

## constructor 属性
```js
"John".constructor                 // 返回函数 String()  { [native code] }
(3.14).constructor                 // 返回函数 Number()  { [native code] }
false.constructor                  // 返回函数 Boolean() { [native code] }
[1,2,3,4].constructor              // 返回函数 Array()   { [native code] }
{name:'John', age:34}.constructor  // 返回函数 Object()  { [native code] }
new Date().constructor             // 返回函数 Date()    { [native code] }
function () {}.constructor         // 返回函数 Function(){ [native code] }
```

