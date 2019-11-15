---
layout: page
title: "Learn Git"
date: 2019-07-21 01:01
---

写的很混乱，复习用。

- 原理
- 命令
- vscode&git

## 版本管理功能

### 回溯

git采用全量存储，类似于Content-addressable的文件系统。

**.git目录**：版本库存储不同版本的文件。

**文件名**：.git中的文件采用SHA1函数值（40位）作为文件名。

**版本库与代码库的文件对应方式**：使用树结构可描述文件名、目录结构及SHA1。类似于快照。

**Commit（提交）**：提交记录快照、记录人信息及上一次提交的SHA1。

![commit](https://raw.githubusercontent.com/Curtails/learninggit/master/image/commit.png)

**暂存区（index）**：介于代码库和版本库中的缓冲区域。

以下为暂存区分别与代码库和版本库的状态比较：

| Code   | 暂存区 | SHA1   | 状态     |
| ------ | ------ | ------ | -------- |
| 在     | 在     | 不匹配 | modified |
| 不存在 | 在     |        | removed  |
| 在     | 不存在 |        | added    |

| 暂存区 | .git   | SHA1   | 状态      |
| ------ | ------ | ------ | --------- |
| 在     | 在     | 不匹配 | modified  |
| 不存在 | 在     |        | missing   |
| 在     | 不存在 |        | untracked |



### 协同

- 分布式版本控制。

- 服务端与本地端都有一份完好的版本库。

push：将本地版本库的代码推送到服务器。

pull：将服务器端的代码拉取到本地。

### 分支

默认分支为master。

分支是指向提交的指针，可以在不同的分支上开发产生了新的提交，开发完成后就可以合并成主干了。

例如有两个分支如下图所示：

![branch1](https://raw.githubusercontent.com/Curtails/learninggit/master/image/branch1.png)

- 两个分支功能开发完成后，开始合并。

- **第一步合并**将master指向weixin并删掉weixin分支。

![branch2](https://raw.githubusercontent.com/Curtails/learninggit/master/image/branch2.png)

- 第二步可以有以下两种情况：
  
  - **merge合并**：三方合并，将微信登陆2、QQ登录2及共同父节点登录，进行一个三分合并。
  
  ![branch3](https://raw.githubusercontent.com/Curtails/learninggit/master/image/branch3.png)
  
  
  
  - **rebase（变基）**：将微博登录2和登录合并成一个新的提交，在QQ登录2后面应用。

![branch4](https://raw.githubusercontent.com/Curtails/learninggit/master/image/branch4.png)



### .git目录中有什么？

- HEAD：描述当前分支HEAD的位置
- index
- objects：存放文件全量历史数据
  - objects中的文件夹名与文件夹中的文件名拼凑起来就是用git add文件的SHA1

## Git常用命令

```

git init					//初始化项目
git add hello.txt			//文件从代码区放到暂存区
git reset HEAD -- hello.txt	//文件从暂存区移出
git cat-file -p SHA1		//查看文件内容
git ls-files --stage		//查看暂存区index内容
git commit -m 'message'		//将暂存区数据提交到仓库，附带信息
git config --global user.name "Your Name"			//设置名字及email地址
git config --global user.email "email@example.com"	
git status					//查看暂存区内容
git diff filename			//查看文件修改
git branch name				//创建分支
git checkout branch			//切换分支
git merge branch			//合并分支
git merge --abort			//恢复merge之前状态
git log						//查看日志
git checkout SHA1			//回溯到SHA1值对应文件
git clone repo				//从服务器端拷贝一个项目
git remote					//查看到所有关联到当前仓库的远程仓库
git remote show name		//查看远程仓库详细信息
git remote add name url		//关联本地仓库和远程仓库name为别名 url为远程仓库地址
git push name branch		//推送代码到远程仓库
git pull name branch		//从远程仓库拉取代码
git remote remove nanme		//删除远程仓库
git stash					//将当前进行中的工作保存到一个暂存区域然后将当前目录回滚到上一次提交
git stash apply				//回滚git stash保存的工作
git stash list				//列出暂存区域保存的进行中的工作

```

## 附录

[fatal: refusing to merge unrelated histories](https://www.cnblogs.com/xiangxinhouse/p/8254120.html)

[Git远程同步Github代码 SSH Key是必须的吗](https://www.jianshu.com/p/ca1f257fdf59)

[配置SSH](https://blog.csdn.net/u011859677/article/details/79249436)

[Updates were rejected because the tip of your current branch is behind](https://blog.csdn.net/zhangkui0418/article/details/82977519)

[用SSH与GIthub建立联系](https://blog.csdn.net/weixin_38317875/article/details/80925750)

[Git warning：LF will be replaced by CRLF](https://blog.csdn.net/starry_night9280/article/details/53207928)

