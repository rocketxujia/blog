---
title: hexo 命令
date: 2016-11-03 23:00:50
tags: 
- hexo
categories: 
- hexo
updated: 
---

# 在线文档

另具体见[hexo doc](https://hexo.io/docs/index.html)
中文版[hexo zn_doc](https://hexo.io/zh-cn/)

# 本地博客操作

**记录第一次从repo下载的最佳执行命令顺序**

- `git clone github_repo` , `git checkout blog-code`  // 从github上下载代码repo，切换到博客源码分支
- `npm i`  // 安装博客依赖模块
- `hexo server`  // 启动博客
- `hexo new post **`  // 开始编辑source文章
- `hexo g`, `hexo deploy` // 部署之前预先生成静态文件到public目录，并拷贝至.deploy_git进行git版本控制, 再提交到origin/master分支
- `hexo clean` // 修改配置文件后，最好执行下， 但要记得CNAME文件回滚

# 新建

- `hexo new "postName"`  // 写文章
- `hexo new page "pageName"` // 新建页面

# 服务

- `hexo generate` # 生成静态页面至public目录
- `hexo server`  # 开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
- `hexo deploy` # 将.deploy目录部署到GitHub

