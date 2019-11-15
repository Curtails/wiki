---
layout: page
title: "渗透-信息收集"
date: 2019-11-04 01:01
---

## 收集域名信息
### Whois查询
`whois baidu.com`
### 备案信息查询
ICP备案查询网 http://www.beianbeian.com
天眼查 http://www.tianyancha.com

## 收集敏感信息
### Google hacking
```
site    
inurl
intext  正文
filetype 文件类型
intitle 标题
link    link:baidu.com 返回所有和baidu.com做了链接的URL
info    查找指定站点的基本信息
cache   快照缓存
```
### BP Repeater
获取服务器server类型及版本 PHP版本信息 

### wappalyzer
chrome 插件

### 乌云公开漏洞表
乌云不在了 还有镜像库 及GitHub存档https://github.com/hanc00l/wooyun_public

## 收集子域名信息
- Layer子域名挖掘机
- subDomainsBrute `python subDomainsBrute.py xxxx.com`
- 搜索引擎枚举 `site:baidu.com`