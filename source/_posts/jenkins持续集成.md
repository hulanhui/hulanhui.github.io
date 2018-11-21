---
title: Jenkins持续集成
date: 2018-11-21 10:00:00
tags: Jenkins
---

docker+jenkins+gitlab+maven 持续集成

#### 安装docker

```
安装：yum install Docker
启动：service docker start
查看：ps -ef|grep docker
```

#### docker 容器中安装 jenkins

```
1、拉jenkins镜像：docker pull jenkins 
2、查看镜像：docker images | grep jenkins 
3、启动镜像：docker run -d --name myjenkins -p 49001:8080 -v /home/jenkins_home:/home/jenkins_home jenkins
    参数说明：
      -d：后台挂起运行
      --name：指定容器名称
      -p：docker内部端口和外部端口映射
      -v：指定挂载目录
      
如果启动后有问题，可以移除容器。
停止：docker stop myjenkins
删除：docker rm myjenkins
rm 和 rmi 区别是：rm是删除容器，rmi是删除镜像
```

#### 初始化首次访问

```
1、访问：http://IP:PORT
2、linux上拷贝文件中的初始密码：/var/jenkins_home/secrets/initialAdminPassword
3、输入密码解锁 jenkins
4、安装plugin插件(git，maven，gitlab，Dingding)
5、设置用户名和密码
```

#### 配置插件

在docker容器里生成ssh秘钥：`ssh-keygen` 

配置gitlab和jenkins授信，这样git才能拉取代码

> 配置gitlab：在gitlab>profile settings>ssh keys 配置生成的公钥，粘贴id_rsa.pub所有内容
>
> 配置jenkins：凭据>添加凭据>
>
> ​	选择类型为：ssh username with private key
>
> ​	username：起个名字
>
> ​	勾选enter directly：粘贴 id_rsa中所有的内容





#### 新建项目









进入docker容器命令：docker exec -it jenkins /bin/bash

查找目录：which|whereis

获取安装包：wget -c https://nginx.org/download/nginx-1.13.1.tar.gz

解压：tar -zxvf nginx-1.13.1.tar.gz 

启动、停止nginx

```
cd /usr/local/nginx/sbin/
./nginx 
./nginx -s quit
ps aux|grep nginx
```



