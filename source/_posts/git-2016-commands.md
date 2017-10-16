---
title: git 命令
date: 2016-07-03 23:00:50
tags: 
- git 命令
categories: 
- git
updated:
comments:
---

# 一、git 关键

## 1.1 命令行中关键词解释
**[分支名]** 指定本地分支
**[远程名]** 指向的远程repo的本地别名
**[远程名]/[分支名]** 指定远程repo下的分支

## 1.2 git project sections

**working directory** 
**staging area**
**git directory**(repository)

{% asset_img section.png "git project sections" %}

**The basic Git workflow goes something like this:**
1) You modify files in your working directory.
2) You stage the files, adding snapshots of them to your staging area.
3) You do a commit, which takes the files as they are in the staging area and stores that snapshot permanently to your Git directory.

## 1.3 File Status LifeCycle
**1) File Untracked** means that Git sees a file you didn’t have in the previous snapshot (commit);
<div>untracked - (git add) -> staged，not unmodified，下图表示有误</div> 
**2) File Tracked** 包括 **unmodified** **modified** **staged**

{% asset_img file_status.png "git file status" %}


# 二、git command

## 查看修改
git diff // what you’ve changed but not yet staged
git diff --staged   // what you’ve staged

## 提交
git commit \-\-amend -am "message"

## 查看提交日志 log
git log -2
git log --graph
git log --pretty=oneline
git log --pretty=format:"%h - %an, %ar : %s"
git log --since=2.weeks
git log --stat  // 会有修改的概况
git log -p // 会有具体diff
git log -- path // 查看指定目录或文件


## 分支的新建与切换
git branch [分支名]
git checkout -b [分支名]
git branch -m [(old-branch)] (new-branch)  // 重命名
git branch --remotes // list remote-tracking
git branch -a  // list both remote-tracking and local branches
git branch -v  // see the last commit on each branch
git branch -d [分支名] // delete the branch
git branch -h
git checkout . // 撤销所有本地修改 

## 合并 merge
git merge [被合并分支名]  
git merge -h

## 衍合 rebase 
git rebase [主分支] [特性分支] 
git rebase -h

## 远程分支
git remote add [远程名] [remote repository url]  // 添加远程repo
git remote rename (old 远程名) (new 远程名)  // 重命名
git remote -v
git remote show [远程名]  // 查看远程repo信息，及
git remote -h

## 同步远程
//  (refspec) 为 (src)[:(dest)]  
// (src) the branch you would want to push
// (dest) the branch on the remote side is updated with this push
git fetch (远程仓库名) \(refspec)  
git fetch -h

## 获取远程到本地
git pull (远程仓库名) (refspec)  // (refspec) 同上
git pull -h

## 推送本地分支
git push (远程仓库名) \(refspec\)  // \(refspec\) 同上
git push (远程仓库名) :(dest)  // 删除远程repo分支dest
git push -h

## 跟踪远程分支
git checkout -b [分支名] [远程名]/[分支名] // 新建一个本地tracking分支，指向跟踪指定的远程分支，并切换到该本地tracking分支，如下提示：
```git
$ git checkout -b serverfix origin/serverfix
Branch serverfix set up to track remote branch serverfix from origin.
Switched to a new branch 'serverfix'
```
git checkout --track [远程名]/[分支名]

