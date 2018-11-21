---
title: Hexo发布文章和同步
date: 2018-09-14 16:20:22
updated: 2018-09-14 16:20:22
tags: Hexo
categories: Hexo
comments: true
---

### Hexo 发布文章和同步

#### 写文章

* `hexo new post “博客名” `，会自动生成到`_posts`目录下

* 写好markdown后放到 `\source\_posts` 目录下

#### 生成以及部署

* `hexo -g d`  去网站就可看到生成的内容

#### 将源文件同步

* git pull git地址 分支名称

* git push git地址 分支名称