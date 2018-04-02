---
title: git 常用命令
author: Don
date: 2018-4-2 22:25:23
tags:
    - Git
categories:
    - Tech
---

工作中最常用的git命令

 查看当前分支改动
```git
git status    
```

### branch

列出当前项目本地分支
``` git
git branch
```

列出当前项目所有分支
``` git
git branch -a
```

基于当前分支创建分支x
```
 git branch x
```

删除本地分支
```
git branch -D x

```

<!-- more -->


创建并切换到分支
```
git branch -b x

```

删除远端分支
```
git push origin --delete x

```

撤销某个文件的改动
```
git checkout x

```

撤销所有文件改动
```
git checkout .

```

撤销git add .操作
```
git reset HEAD .

```

push所有分支到远端
```
git push --all

```

 在本地创建与远端对应的名称一样的分支
 ```
git checkout --track origin/x

 ```

  在本地创建与远端对应的名称为xx的分支
  ```
git checkout --track xx origin/x

  ```

### stash

暂存当前分支的改动
```
git stash

```

  恢复当前分支暂存的改动
  ```
git stash pop

  ```

  ### tag

查看当前项目的所有tag
```
git tag

```

查看某个tag信息
```
git show tagName

```

  给当前分支的当前commit打标签，方便以后版本回退
  ```
 git tag -a v1.1.1 -m "publish version 1.1.1"

  ```

 回退到某个commit对应的版本
 ```
 git reset --hard xx (xx代表某次commit的hash值的前6位)

 ```

 提交所有tag到仓库
 ```
 git push --all tags

```

 提交某个tag到远端
 ```
git push origin tagName

 ```

删除本地tag
```
git tag -d tagName

```

 删除远端tag
 ```
git push origin :refs/tags/xx

 ```

 ### remote 

查看当前git仓库远端地址
```
git remote -v

```

删除当前git仓库的远端地址
```
git remote rm origin

```

为本地仓库添加远端地址
```
git remote add origin git@xxx

```

修改当前仓库的远程地址
```
git remote set-url origin git@xxx

```



