---
title: Git常用命令
date: 2019-01-07 22:44:17
tags: 工具
---

* 初始化

```
git init
```

所在文件夹在初始化后具有`git`仓库管理功能，若要取消具有`git`仓库管理功能的文件夹，删除用于仓库管理的`.git`文件夹`rm -rf .git`（或`rm -fr .git`）即可。

* 查看项目地址：

```
git remote -v
```

* 已有分支下创建新分支：

```
git checkout -b temp v1.0.0
(temp新分支名，v1.0.0已有分支名)
```

* 版本回退：

```
git reset --hard commit_id
回退到指定的commit_id版本
```

* 查看分支

```
git branch 查看本地分支，并指示当前所在分支
git branch -a 查看所有分支，包括本地分支和远程分支，并指示当前所在分支
```

* 删除分支

```
删除分支时，当前分支不要处于将要被删除的分支上
git branch -d 将要被删除的分支名称
git branch -D 将要被删除的分支名称 该操作会强删分支（谨慎操作!!!）
```

* 在已有的分支上创建分支

```
git checkout -b 新分支
会在当前分支下创建新分支，并切换到新分支上
```

* 查看修改的详细信息

```
针对具体文件，查看修改的详细信息
git log --pretty=oneline 文件名
git show 356f6def9d3fb7f3b9032ff5aa4b9110d4cca87e（文件提交时生成的哈希码）（可以只拷贝该哈希码的前面几位，最少7位，即可查看对应的文件）
```