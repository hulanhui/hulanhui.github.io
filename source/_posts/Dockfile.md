---
title: Dockerfile
date: 2018-11-21 10:00:00
tags: Docker
---

### Dockfile

Dockfile是一个用于编写docker镜像生成过程的文件，其有特定的语法。在一个文件夹中，如果有一个名字为Dockfile的文件，其内容满足语法要求，在这个文件夹路径下执行命令:

`docker build --tag name:tag .`，就可以按照描述构建一个镜像了。

name是镜像的名称，

tag是镜像的版本或者是标签号，

不写就是lastest。注意后面有一个空格和点。 



Dockerfile详解 Dockerfile 分为四部分：

### 命令

#### **FROM **

**指定基础镜像，要在哪个镜像建立**

> 格式为 `FROM <image> 或FROM <image>:<tag>`

说明：第一个指令必须是FROM了，其指定一个构建镜像的基础源镜像，如果本地没有就会从公共库中拉取，没有指定镜像的标签会使用默认的latest标签，可以出现多次，如果需要在一个Dockerfile中构建多个镜像。 



#### **MAINTAINER**

**描述镜像的创建者，名称和邮箱 **

> 格式为 `MAINTAINER <name> <email>`



#### **RUN**

**在镜像中要执行的命令**

> 格式为 `RUN <command> 或 RUN ["executable", "param1", "param2"]`

说明：前者将在 shell 终端中运行命令，即 /bin/bash -c ；

后者则使用 exec 执行。指定使用其它终端可以通过第二种方式实现，

例如 `RUN [“/bin/bash”, “-c”,”echo hello”]` 。



#### **WORKDIR**

**指定当前工作目录，相当于 cd**

> 格式为 `WORKDIR /path/to/workdir`

说明：为后续的 RUN 、 CMD 、 ENTRYPOINT 指令配置工作目录。
可以使用多个 WORKDIR 指令，后续命令如果参数是相对路径，则会基于之前命令指定的路径。

```
示例：
WORKDIR /a
WORKDIR b
WORKDIR c
RUN pwd
则最终路径为 /a/b/c
```



#### **EXPOSE**

**指定容器要打开的端口**

> 格式为 `EXPOSE <port> [<port>...]`

说明：告诉 Docker 服务端容器暴露的端口号，供互联系统使用。在启动容器时需要通过 -P，Docker 主机会自动分配一个端口转发到指定的端口。在docker run -p的时候生效。 



#### **ENV**

**定义环境变量**

格式为 `ENV <key> <value>` 。 指定一个环境变量，会被后续 RUN 指令使用，并在容器运行时保持。
例如

> `ENV PATH /usr/local/nginx/sbin:$PATH`

说明：设置容器的环境变量，可以让其后面的RUN命令使用，容器运行的时候这个变量也会保留。 



#### **COPY**

**复制本地主机的 （为 Dockerfile 所在目录的相对路径）到容器中的**

> 格式为 COPY <src> <dest> 

说明：COPY除了不能自动解压，也不能复制网络文件。其它功能和ADD相同。 



#### **ADD**

**相当于 COPY，但是比 COPY 功能更强大**

> 格式为 `ADD <src> <dest>`

说明：该命令将复制指定的 到容器中的 。 其中 可以是Dockerfile所在目录的一个相对路径；也可以是一个 URL；还可以是一个 tar 文件，复制进容器会自动解压。

复制本机文件或目录或远程文件，添加到指定的容器目录，支持GO的正则模糊匹配。路径是绝对路径，不存在会自动创建。如果源是一个目录，只会复制目录下的内容，目录本身不会复制。ADD命令会将复制的压缩文件夹自动解压，这也是与COPY命令最大的不同。 



#### **VOLUME**

**挂载目录**

> 格式为`VOLUME ["/data"]`

说明：创建一个可以从本地主机或其他容器挂载的挂载点，一般用来存放数据库和需要保持的数据等。

#### **USER**

> 格式为 `USER daemon`

说明：指定运行容器时的用户名或 UID，后续的 RUN 也会使用指定用户。当服务不需要管理员权限时，可以通过该命令指定运行用户。并且可以在之前创建所需要的用户，例如： RUN useradd -s /sbin/nologin -M www。



#### **ENTRYPOINT**

两种格式：

```
ENTRYPOINT ["executable", "param1", "param2"]
ENTRYPOINT command param1 param2 （shell中执行）
```

配置容器启动后执行的命令，并且不可被 docker run 提供的参数覆盖。每个 Dockerfile 中只能有一个 ENTRYPOINT ，当指定多个时，只有最后一个起效。

#### **CMD**

支持三种格式

```
CMD ["executable","param1","param2"] 使用 exec 执行，推荐方式；
CMD command param1 param2 在 /bin/bash 中执行，提供给需要交互的应用；
CMD ["param1","param2"] 提供给 ENTRYPOINT 的默认参数；
```

指定启动容器时执行的命令，每个 Dockerfile 只能有一条 CMD 命令。如果指定了多条命令，只有最后一条会被执行。如果用户启动容器时候指定了运行的命令，则会覆盖掉 CMD 指定的命令。



#### **ONBUILD**

**在构建本镜像时不生效，在基于此镜像构建镜像时生效**

> 格式为 `ONBUILD [INSTRUCTION]`

配置当所创建的镜像作为其它新创建镜像的基础镜像时，所执行的操作指令。

ENTRYPOINT 和 CMD 的区别：ENTRYPOINT 指定了该镜像启动时的入口，CMD 则指定了容器启动时的命令，当两者共用时，完整的启动命令像是 ENTRYPOINT + CMD 这样。使用 ENTRYPOINT 的好处是在我们启动镜像就像是启动了一个可执行程序，在 CMD 上仅需要指定参数；另外在我们需要自定义 CMD 时不容易出错。

使用 CMD 的 Dockerfile：

```
[root@sta2 test]# cat Dockerfile 
FROM mysql
CMD ["echo","test"]
```

使用 ENTRYPOINT 的 Dockerfile：

```
[root@sta2 entrypoint]#  cat  Dockerfile 
FROM mysql
ENTRYPOINT ["echo","test"]
```

结论：ENTRYPOINT 不能覆盖掉执行时的参数，CMD 可以掉覆盖默认的参数。