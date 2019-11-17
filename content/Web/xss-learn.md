---
title: "XSS学习"
data: 2019-11-16 03:33
---
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

## 存储型xss攻击
