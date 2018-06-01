---
title: git命令记录
---

# 说明
根据廖雪峰的[git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)整理，精简版

# 用户名相关
查看当前电脑的用户名和邮箱
```
git config user.name
git config user.email
```
修改当前电脑的用户名和邮箱
```
git config --global user.name "xxx"
git config --global user.email "xxx"
```

# 时光机穿梭
## 工作区和暂存区
将工作区的内容提交到暂存区，此操作完成后可以使用`git status`查看状态。
```
git add ...
```
将暂存区的内容提交到**当前分支**，此操作会导致暂存区清空
```
git commit ...
```
查看**当前分支**可以使用命令
```
git branch
```
当提交以后如果想查看**工作区**和**版本库**之间的区别，可以使用指令
```
git diff HEAD -- xxx.txt
```
## 删除已经做出的修改
如果工作区中的内容已经做出了修改可以使用如下指令进行撤销
```
git checkout -- xxx.txt
```
且这里要注意区分两种情况：
1. 已经做出了修改，但是还没有提交到暂存区，输入上述命令会将工作区中的内容全部撤销。换句话说，就是恢复到了使用`git commit`之前的状态
2. 提交到了暂存区以后还做出了修改，输入上述命令会恢复到提交到暂存区之前的状态。换句话说，就是恢复到了`git add`之前的状态。

如果工作区中的内容提交到了暂存区，但是还没有提交到版本库，那么可以使用下面的指令撤销，此操作会清除暂存区中对应的文件状态。
```
git reset HEAD xxx.txt
```
这个命令可以这么读：
> 将文件xxx.txt恢复到HEAD所指向的版本库的状态
> 
又由于`git reset HEAD xxx.txt`中的`HEAD`参数可以换成`HEAD^`、`HEAD^^`等版本库的编号，所以这个命令又有**回退版本**的作用。

如果不幸将暂存区中的内容添加到了版本库，那么只能够使用版本回退操作来完成了，即根据上面的说法，使用：
```
git reset HEAD^ xxx.txt
```

如果更不幸，你将本地最新的版本库提交到了远程分支，就挂了...

# 分支管理
## 创建与合并分支
> 一开始的时候，分支是一条线，`master`指向最新的提交，`HEAD`指向的是`master`，更准确的说法是，`HEAD`指向的是当前分支的最新提交，当前分支不一定是`master`。

如果当前分支是`master`，需要开一条新的分支`dev`来完成任务，完成以后要进行分支的合并，然后删除`dev`分支，那么操作如下，设此时的分支为`master`:
```
// create an branch dev and switch to it
git checkout -b dev

// do something on branch dev
git add modify.txt
git commit -m "..."

// finish the work on branch dev

// switch to master
git checkout master

// merge branch master with branch dev
git merge dev

// delete the branch dev
git branch -d dev
```

## 解决冲突
解决冲突并不是一个什么难的问题，这里就贴两张图来说明冲突之前和冲突之后的分支状态，转自。
这是冲突发生时的冲突状态
![](http://p8gk4u1ta.bkt.clouddn.com/18-6-1/83945159.jpg)
这是冲突解决了以后的状态。
![](http://p8gk4u1ta.bkt.clouddn.com/18-6-1/58547916.jpg)

## 分支管理策略
根据廖雪峰的博客内容，这里记录一下通用的分支管理状态
1. `master`是最稳定的分支，这条分支仅仅作为版本的发布，其他任何时候不能够动
2. `dev`分支为开发分支，这条分支从`master`的起点分出来，并向`master`提交版本。
3. 所有的个人开发分支全部由`dev`的起点分支分离出来，开发完成以后向`dev`分支提交。
如图：
![](http://p8gk4u1ta.bkt.clouddn.com/18-6-1/40940055.jpg)

## 多人协作
如果要推送`master`分支，使用如下命令：
```
git push origin master
```
如果要推送其他的分支，比如`dev`，使用如下命令：
```
git push origin dev
```
前面介绍了，如果要切换并创建一个分支，可以使用：
```
git checkout -b newbranch
```
但是如果要复制远程主机中的分支到本地，那么需要使用：
```
git checkout -b newbranch origin/newbranch
```
当需要从远程指定分支pull内容到本地指定分支的时候，使用：
```
git branch --set-upstream-to=origin/dev dev
```
然后再使用：
```
git pull
```
既然是`pull`下来的内容，出现了冲突也很正常，使用`git status`来查看冲突，此时需要明确一个概念---工作区是一个可以看得见的一个文件夹，而暂存区和版本库保存的是修改，所以不是直接可以看见的东西，所以可以存在工作区的内容和版本库的内容不同的情况，此时需要手动合并。