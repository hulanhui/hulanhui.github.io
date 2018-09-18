---
title: Hexo&Github搭建自己的博客
date: 2018-09-14 16:20:22
tags: Hexo
---



### **Hexo&Github搭建自己的博客**

#### 1、安装：[Node.Js](http://nodejs.org/)

#### 2、安装：[Git](http://git-scm.com/)

#### 3、[Github](https://github.com) 账户注册和新建项目 

#### 4、安装 [Hexo](https://hexo.io/zh-cn/docs/)

```
 安装好了nodejs和git及github后
 打开cmd: 切换到你的 <folder> 上级目录，执行以下命令：
 
 $ 安装hexo：npm install -g hexo-cli
 $ 检查是否成功：hexo -v
 $ 初始化文件夹：hexo init <folder>
 $ 进入文件夹：cd <folder>
 $ 安装依赖：npm install
 $ 生成文件：hexo g
 $ 启动服务：hexo s
 $ 访问：http://localhost:4000
 $ 切换端口(如果端口被占用)：hexo server -p 端口号
```

注：folder为你的bolg文件夹名称

到这步一步，项目能本地访问啦，接下来同步到github上让外网能直接访问！

---

#### 5、使用git 生成ssh key 

#### 6、github 配置 ssh  key

#### 7、npm安装插件：hexo-deployer-git

```js
npm install hexo-deployer-git --save
```

#### 8、修改根目录下：_config.yml 配置文件

```java
deploy:
  type: git
  repo: git@github.com:youself/youself.github.io.git
  branch: master
  #说明：
  #repo: 库（Repository）地址，github新建的项目地址
  #branch: 分支名称
  #message: 自定义提交信息 (默认为 Site updated: {{ now('YYYY-MM-DD HH:mm:ss') }})
```

#### 9、测试添加ssh是否成功

`ssh -T git@github.com`

#### 10、新建一篇博客

`hexo new post "博客名"`

#### 11、生成及部署

`hexo d -g `

12、部署成功后访问你的地址：http://用户名.github.io。那么将看到生成的文章 ！



参考安装文章：

[官网安装文档](https://hexo.io/zh-cn/docs/)

[使用Hexo+Github一步步搭建属于自己的博客（基础）](https://www.cnblogs.com/fengxiongZz/p/7707219.html)