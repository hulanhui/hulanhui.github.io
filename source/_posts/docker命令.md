---
title: Docker命令
date: 2018-11-21 10:00:00
tags: Docker
---



#### 简介

> Docker是一个开源的引擎，可以轻松的为任何应用创建一个轻量级的、可移植的、自给自足的容器

#### 使用场景

- web应用的自动化打包和发布；
- 自动化测试和持续集成、发布；
- 在服务型环境中部署和调整数据库或其他的后台应用；
- 从头编译或者扩展现有的OpenShift或Cloud Foundry平台来搭建自己的PaaS环境。

#### 安装docker

* Docker系统有两个程序：docker服务端和docker客户端
* Docker有很多种安装的选择，我们推荐您在Ubuntu下面安装，因为docker是在Ubuntu下面开发的，安装包测试比较充分，可以保证软件包的可用性。Mac, windows和其他的一些linux发行版本无法原生运行Docker，可以使用虚拟软件创建一个ubuntu的虚拟机并在里面运行docker。

#### 常用命令

* 启动：service docker start
* 停止：service docker stop
* 是否运行：ps aux | grep docker

#### docker命令

```xml
显示docker版本：docker version
显示docker系统信息：docker info 
生命周期类
* 运行：docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
    使用docker镜像nginx:latest以后台模式启动一个容器,并将容器命名为mynginx。
    docker run --name mynginx -d nginx:latest
    使用镜像nginx:latest以后台模式启动一个容器,并将容器的80端口映射到主机随机端口。
    docker run -P -d nginx:latest
    使用镜像 nginx:latest，以后台模式启动一个容器,将容器的 80 端口映射到主机的 80 端口,主机的目录 		/data 映射到容器的 /data。
    docker run -p 80:80 -v /data:/data -d nginx:latest
    绑定容器的 8080 端口，并将其映射到本地主机 127.0.0.1 的 80 端口上。
    docker run -p 127.0.0.1:80:8080/tcp ubuntu bash
    使用镜像nginx:latest以交互模式启动一个容器,在容器内执行/bin/bash命令。
    runoob@runoob:~$ docker run -it nginx:latest /bin/bash
* 启动：docker start [OPTIONS] CONTAINER [CONTAINER...]
* 停止：docker stop [OPTIONS] CONTAINER [CONTAINER...]
* 重启：docker restart [OPTIONS] CONTAINER [CONTAINER...]
* 杀掉运行中的容器：docker kill [OPTIONS] CONTAINER [CONTAINER...]
* 删除一个或多少容器：docker rm [OPTIONS] CONTAINER [CONTAINER...]
* 暂停容器中所有的进程: docker pause [OPTIONS] CONTAINER [CONTAINER...]
* 恢复容器中所有的进程: docker unpause [OPTIONS] CONTAINER [CONTAINER...]
* 创建一个新的容器但不启动它: docker create [OPTIONS] IMAGE [COMMAND] [ARG...]
    使用docker镜像nginx:latest创建一个容器,并将容器命名为myrunoob
    docker create  --name myrunoob  nginx:latest
* 在运行的容器中执行命令: docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
    在容器mynginx中以交互模式执行容器内/root/runoob.sh脚本
    docker exec -it mynginx /bin/sh /root/runoob.sh

容器操作类
* 列表：docker ps [OPTIONS]
	列出所有在运行的容器信息: docker ps
	列出最近创建的5个容器信息: docker ps -n 5 
	列出所有创建的容器ID: docker ps -a -q
* 获取容器/镜像的元数据：docker inspect [OPTIONS] NAME|ID [NAME|ID...]
	获取镜像mysql:5.6的元信息：docker inspect mysql:5.6
* 查看容器中运行的进程信息，支持 ps 命令参数：docker top [OPTIONS] CONTAINER [ps OPTIONS]
	查看容器mymysql的进程信息：docker top mymysql
* 获取容器的日志: docker logs [OPTIONS] CONTAINER
	跟踪查看容器mynginx的日志输出: docker logs -f myngin
* 列出指定的容器的端口映射: docker port [OPTIONS] CONTAINER [PRIVATE_PORT[/PROTO]]
	查看容器mynginx的端口映射情况: docker port mymysql

镜像仓库类
* 登陆到一个Docker镜像仓库 docker login [OPTIONS] [SERVER]
* 登出一个Docker镜像仓库docker logout [OPTIONS] [SERVER]
	登陆到Docker Hub: docker login -u 用户名 -p 密码
	登出Docker Hub: docker logout
* 从镜像仓库中拉取或者更新指定镜像：docker pull [OPTIONS] NAME[:TAG|@DIGEST]
* 将本地的镜像上传到镜像仓库,要先登陆到镜像仓库：docker push [OPTIONS] NAME[:TAG]
	上传本地镜像myapache:v1到镜像仓库中: docker push myapache:v1
* 从Docker Hub查找镜像：docker search [OPTIONS] TERM
	从Docker Hub查找所有镜像名包含java，并且收藏数大于10的镜像: docker search -s 10 java

本地镜像管理类
* 列出本地镜像: docker images [OPTIONS] [REPOSITORY[:TAG]]
* 删除本地一个或多少镜像: docker rmi [OPTIONS] IMAGE [IMAGE...]
* 标记本地镜像，将其归入某一仓库: docker tag [OPTIONS] IMAGE[:TAG] [REGISTRYHOST/][USERNAME/]NAME[:TAG]
* docker build 命令用于使用 Dockerfile 创建镜像： docker build [OPTIONS] PATH | URL | -
	使用当前目录的 Dockerfile 创建镜像，标签为 runoob/ubuntu:v1
	docker build -t runoob/ubuntu:v1
	使用URL github.com/creack/docker-firefox 的 Dockerfile 创建镜像
	docker build github.com/creack/docker-firefox
	也可以通过 -f Dockerfile 文件的位置：
	docker build -f /path/to/a/Dockerfile 
	在 Docker 守护进程执行 Dockerfile 中的指令前，首先会对 Dockerfile 进行语法检查，有语法错误时会返回：docker build -t test/myapp
* 查看指定镜像的创建历史: docker history [OPTIONS] IMAGE
* 将指定镜像保存成 tar 归档文件: docker save [OPTIONS] IMAGE [IMAGE...]
* 从归档文件中创建镜像: docker import [OPTIONS] file|URL|- [REPOSITORY[:TAG]]
	从镜像归档文件my_ubuntu_v3.tar创建镜像，命名为runoob/ubuntu:v4
	docker import  my_ubuntu_v3.tar runoob/ubuntu:v4  


run、start、stop、restart OPTIONS说明：
-a stdin: 指定标准输入输出内容类型，可选 STDIN/STDOUT/STDERR 三项；
-d: 后台运行容器，并返回容器ID；
-i: 以交互模式运行容器，通常与 -t 同时使用；
-p: 端口映射，格式为：主机(宿主)端口:容器端口
-t: 为容器重新分配一个伪输入终端，通常与 -i 同时使用；
--name="nginx-lb": 为容器指定一个名称；
--dns 8.8.8.8: 指定容器使用的DNS服务器，默认和宿主一致；
--dns-search example.com: 指定容器DNS搜索域名，默认和宿主一致；
-h "mars": 指定容器的hostname；
-e username="ritchie": 设置环境变量；
--env-file=[]: 从指定文件读入环境变量；
--cpuset="0-2" or --cpuset="0,1,2": 绑定容器到指定CPU运行；
-m :设置容器使用内存最大值；
--net="bridge": 指定容器的网络连接类型，支持 bridge/host/none/container: 四种类型；
--link=[]: 添加链接到另一个容器；
--expose=[]: 开放一个端口或一组端口；

kill OPTIONS说明：
-s :向容器发送一个信号

rm OPTIONS说明：
-f :通过SIGKILL信号强制删除一个运行中的容器
-l :移除容器间的网络连接，而非容器本身
-v :-v 删除与容器关联的卷

exec OPTIONS说明：
-d :分离模式: 在后台运行
-i :即使没有附加也保持STDIN 打开
-t :分配一个伪终端

ps OPTIONS说明：
-a :显示所有的容器，包括未运行的。
-f :根据条件过滤显示的内容。
--format :指定返回值的模板文件。
-l :显示最近创建的容器。
-n :列出最近创建的n个容器。
--no-trunc :不截断输出。
-q :静默模式，只显示容器编号。
-s :显示总的文件大小。

inspect OPTIONS说明：
-f :指定返回值的模板文件。
-s :显示总的文件大小。
--type :为指定类型返回JSON。

logs OPTIONS说明：
-f : 跟踪日志输出
--since :显示某个开始时间的所有日志
-t : 显示时间戳
--tail :仅列出最新N条容器日志

pull OPTIONS说明：
-a :拉取所有 tagged 镜像
--disable-content-trust :忽略镜像的校验,默认开启

push OPTIONS说明：
--disable-content-trust :忽略镜像的校验,默认开启

search OPTIONS说明：
--automated :只列出 automated build类型的镜像；
--no-trunc :显示完整的镜像描述；
-s :列出收藏数不小于指定值的镜像。

images OPTIONS说明：
-a :列出本地所有的镜像（含中间映像层，默认情况下，过滤掉中间映像层）；
--digests :显示镜像的摘要信息；
-f :显示满足条件的镜像；
--format :指定返回值的模板文件；
--no-trunc :显示完整的镜像信息；
-q :只显示镜像ID。

rmi OPTIONS说明：
-f :强制删除；
--no-prune :不移除该镜像的过程镜像，默认移除；

build OPTIONS说明：
--build-arg=[] :设置镜像创建时的变量；
--cpu-shares :设置 cpu 使用权重；
--cpu-period :限制 CPU CFS周期；
--cpu-quota :限制 CPU CFS配额；
--cpuset-cpus :指定使用的CPU id；
--cpuset-mems :指定使用的内存 id；
--disable-content-trust :忽略校验，默认开启；
-f :指定要使用的Dockerfile路径；
--force-rm :设置镜像过程中删除中间容器；
--isolation :使用容器隔离技术；
--label=[] :设置镜像使用的元数据；
-m :设置内存最大值；
--memory-swap :设置Swap的最大值为内存+swap，"-1"表示不限swap；
--no-cache :创建镜像的过程不使用缓存；
--pull :尝试去更新镜像的新版本；
--quiet, -q :安静模式，成功后只输出镜像 ID；
--rm :设置镜像成功后删除中间容器；
--shm-size :设置/dev/shm的大小，默认值是64M；
--ulimit :Ulimit配置。
--tag, -t: 镜像的名字及标签，通常 name:tag 或者 name 格式；可以在一次构建中为一个镜像设置多个标签。
--network: 默认 default。在构建期间设置RUN指令的网络模式

history OPTIONS说明：
-H :以可读的格式打印镜像大小和日期，默认为true；
--no-trunc :显示完整的提交记录；
-q :仅列出提交记录ID。

import OPTIONS说明：
-c :应用docker 指令创建镜像；
-m :提交时的说明文字；

搜索docker hub镜像：docker search tutorial
下载容器镜像：docker pull 用户名/镜像名
* 示例：docker pull learn/tutorial
* 镜像都是按照用户名/镜像名的方式来存储的
* 有一组比较特殊的镜像，比如ubuntu这类基础镜像，经过官方的验证，值得信任，可以直接用镜像名来检索到
查看本地镜像：docker images|grep tomcat



安装程序：在容器里面安装一个简单的程序(ping)
* 示例：docker run learn/tutorial apt-get install -y ping
* 我们之前下载的tutorial镜像是基于ubuntu的
* 所以你可以使用ubuntu的apt-get命令来安装ping程序：apt-get install -y ping。
* apt-get 命令执行完毕之后，容器就会停止，但对容器的改动不会丢失。

保存对容器的修改：当你对某一个容器做了修改之后（通过在容器中运行某一个命令），可以把对容器的修改保存下来，这样下次可以从保存后的最新状态运行该容器。docker中保存状态的过程称之为committing，它保存的新旧状态之间的区别，从而产生一个新的版本。
* 示例：docker commit 698 learn/ping
* 首先使用docker ps -l 命令获得安装完ping命令之后容器的id。然后把这个镜像保存为learn/ping。
* 你需要指定要提交保存容器的ID。(译者按：通过docker ps -l 命令获得)
* 无需拷贝完整的id，通常来讲最开始的三至四个字母即可区分。（译者按：非常类似git里面的版本号)

运行：docker run 容器镜像名
* 示例：docker run learn/tutorial echo "hello word"
* 通过docker run命令可以启动某一个镜像，并运行一个命令。
* docker run命令有两个参数，一个是镜像名，一个是要在镜像中运行的命令。

检查运行中的镜像：
* 示例： docker inspect efe
* 使用docker ps命令可以查看所有正在运行中的容器列表
* 使用docker inspect命令我们可以查看更详细的关于某一个容器的信息。
* 可以使用镜像id的前面部分，不需要完整的id。


运行新的镜像：对一个镜像提交修改之后，就可以运行它里面新安装的命令了。
示例：docker run lean/ping ping www.google.com
* 一定要使用新的镜像名learn/ping来运行ping命令。(译者按：最开始下载的learn/tutorial镜像中是没有ping命令的)

发布自己的镜像：我们可以将其发布到官方的索引网站。还记得我们最开始下载的learn/tutorial镜像吧，我们也可以把我们自己编译的镜像发布到索引页面，一方面可以自己重用，另一方面也可以分享给其他人使用。
* 示例：docker push learn/ping
* docker images命令可以列出所有安装过的镜像。
* docker push命令可以将某一个镜像发布到官方网站。
* 你只能将镜像发布到自己的空间下面。这个模拟器登录的是learn帐号。
```



