---
title: "git-rebase"
image: 
  path: /imgs/ 
  thumbnail: /imgs/ 
categories:
  - Notes
tags:
  - Notes
excerpt_separator: <!--more-->
last_modified_at: 2020-09-17T06:00:23-08:00
---

https://blog.csdn.net/sky8336/article/details/108237952

git 更改某个提交内容和将当前改动追加到某次commit 上
尤其适用于gerrit 上的CL 未submit 之前，有多个patch时的多个功能点并行开发。

rebase 前，可以创建一个分支，用于rebase:
git checkout -b rebase_tmp

直接更改某次提交(改动某个指定的commit)
将HEAD移到需要更改的commit上:
git rebase -i 0bdf89^
找到需要更改的commit, 将行首的pick改成edit, 按esc, 输入:wq退出
（弹出的交互式界面中显示的commit 信息，与git log 显示的顺序相反，即父节点在上显示）
更改文件
使用git add 改动的文件添加改动文件到暂存
使用git commit --amend追加改动到第一步中指定的commit上
使用git rebase --continue移动HEAD到最新的commit处
解决冲突:
编辑冲突文件, 解决冲突
git add .
git commit --amend
解决冲突之后再执行git rebase --continue
将工作空间中的改动追加到某次提交上
**note:**第0步和第2步（加粗）与上面步骤不同，其余步骤相同

保存工作空间中的改动git stash
将HEAD移到需要更改的commit上:
git rebase f744c32^ --interactive
找到需要更改的commit, 将行首的pick改成edit, 按esc, 输入:wq退出
执行命令git stash pop
使用git add 改动的文件添加改动文件到暂存
使用git commit --amend追加改动到第一步中指定的commit上
使用git rebase --continue移动HEAD到最新的commit处
解决冲突:
编辑冲突文件, 解决冲突
git add .
git commit --amend
解决冲突之后再执行git rebase --continue
这样处理的优点
如果branchB分支需要branchA分支上的某个功能, 只需要找到这个功能的惟一的一个提交记录即可, 就不需要在很多commit之中寻找这个功能点的相关提交记录. 更改合并之后再移动功能点, 就简单了许多, 执行找到功能点的惟一一个提交记录, 让后使用git cherry-pick commit-hash即可,