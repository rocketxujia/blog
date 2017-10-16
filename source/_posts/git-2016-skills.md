---
title: Git的几个提供效率的技巧
layout: fasle
date: 2016-07-15 23:22:36
tags: 
- git 技巧
categories: 
- git
updated:
comments:
---

## 1. Git自动补全
你可以启用Git的自动补全功能，完成这项工作仅需要几分钟。
````shell
cd ~
curl https://raw.github.com/git/git/master/contrib/completion/git-completion.bash -o ~/.git-completion.bash
````
然后，添加下面几行到你的 ~/.bash_profile 文件中：
````
# Git
source ~/.git-completion.bash
````
这时候你就可以使用`tab`键，进行命令输入的自动智能补全




