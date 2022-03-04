Docker

# Docker学习

* Docekr概述
* Docker安装
* Doker命令
  * 镜像命令
  * 容器命令
  * 操作命令
  * ...
* Doker镜像
* 容器数据卷
* DockerFile
* Docker网络原理()
* IDEA整合Docker
* Docker Compose
* Docker Swarm 
* CI\CD Jenkins

# Docekr地址

官网：[Empowering App Development for Developers | Docker](https://www.docker.com/)

文档地址：[Docker Documentation | Docker Documentation](https://docs.docker.com/)

仓库地址：[Docker Hub Container Image Library | App Containerization](https://hub.docker.com/)

# Docker概述

比较Docker和传统虚拟机技术的不同：

* 传统虚拟机，虚拟出一条硬件，运行一个完整的操作系统，在这个系统上安装运行软件、
* 容器内的应用直接运行在宿主机的内容，容器没有自己的内核，也没有虚拟我们的硬件，所以就轻便了
* 每个容器之间相互隔离，每个容器内都有一个属于自己的文件系统，互不影响

> DevOps(开发、运维)

**应用更快速的交付和部署**

传统：一堆帮助文档，安装程序

Docker：意见运行打包镜像发布测试，一键运行

**更便捷的升级和扩缩容**

使用Docker之后，部署应用和搭积木一样

**更简单的系统运维**

在容器化之后，我们的开发，测试环境都是高度一致的

**更高效的计算资源利用**

Docker是内核级别的虚拟化，可以在一个物理机上运行很多的容器实例！服务器的性能可以被压榨到极致  

# Docker安装

## Docker的基本组成

![img](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fcdn.learnku.com%2Fuploads%2Fimages%2F202003%2F18%2F22215%2Fv7N8wg4sy3.png%21large&refer=http%3A%2F%2Fcdn.learnku.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1648907607&t=0f2c40ffec6ff4440163bd7650cff630)

**镜像（image）:** 

docker镜像好比是一个模板，可以通过模板来创建容器服务，tomcat镜像==> run ==>

tomcat01容器(提供服务),通过这个镜像可以创建多个容器(最终服务运行或者项目运行就是在容器中的)。

**容器（container）:**

Docker利用容器技术，独立运行一个或者一个组应用，通过镜像来创建

启动，停止，删除，基本命令！

目前可以把这个容器理解为就是一个建议的linux系统

**仓库（repository）：**

仓库就是存放镜像的地方

分为公有仓库和私有仓库

Docker Hub（默认国外的）

阿里云...都有容器服务(配置镜像加速)

## 安装Docker

> 环境准备

1. 需要linux命令基础
2. centos7
3. 使用xshell连接远程服务器进行操作

> 环境查看

查看系统版本

> 安装

参照官方帮助文档：[Install Docker Engine on Ubuntu | Docker Documentation](https://docs.docker.com/engine/install/ubuntu/)

## 回顾HelloWorld流程

![image-20220304133030006.png](https://s2.loli.net/2022/03/04/UQ1W3AcfDdRONKy.png)

<img src="https://s2.loli.net/2022/03/04/UR9CfSyomI3Ttqs.jpg" alt="屏幕截图 2022-03-04 135714.jpg" style="zoom:80%;" />

## 底层原理

**Docker是怎么工作的?**

Docker是一个Client - Server结构的系统，Docker的守护进程运行在主机上。通过Socket从客户端访问

Docker接收到Docker-client的指令，就会执行这个命令

**Docker为什么比vm快**

<img src="https://www.freesion.com/images/783/5d9e93eaef0d2a0c567330a47f2ba25f.png" alt="查看源图像" style="zoom:50%;" />

1. Docker有着比虚拟机更少的抽象层
2. Docker利用的是宿主机的内核，vm需要的是Guest OS。
3. 新建一个容器的时候，Docker不需要像虚拟机一样重新加载一个内核，避免引导，虚拟机是加载guest os，分钟级别的，而Docker是利用宿主机的操作系统省略了复杂的过程，秒级

# Docker常用命令

## 帮助命令

```shell
docker version #显示docker版本信息
docker info #显示docker的系统信息，包括镜像和容器
docker 命令 --help #万能命令
```

帮助文档的地址：[Use the Docker command line | Docker Documentation](https://docs.docker.com/engine/reference/commandline/cli/)

## 镜像命令

**docker images 查看主机上的所有镜像**

```shell
ubuntu@VM-16-8-ubuntu:~$ docker images
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
ubuntu        latest    ba6acccedd29   4 months ago   72.8MB
hello-world   latest    feb5d9fea6a5   5 months ago   13.3kB
ubuntu        12.10     3e314f95dcac   7 years ago    172MB

#解释
REPOSITORY 镜像的仓库源
TAG 镜像的标签
IMAGE ID 镜像的id
CREATED 创建的时间
SIZE 镜像大小
#可选项
  -a, --all             Show all images (default hides intermediate images)
      --digests         Show digests
  -f, --filter filter   Filter output based on conditions provided
      --format string   Pretty-print images using a Go template
      --no-trunc        Don't truncate output
  -q, --quiet           Only show image IDs
```

**docker search 搜索镜像**

```shell
ubuntu@VM-16-8-ubuntu:~$ docker search mysql
NAME                             DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql                            MySQL is a widely used, open-source relation…   12206     [OK]       
mariadb                          MariaDB Server is a high performing open sou…   4688      [OK]       
mysql/mysql-server               Optimized MySQL Server Docker images. Create…   907                  [OK]
percona                          Percona Server is a fork of the MySQL relati…   570       [OK]       
phpmyadmin                       phpMyAdmin - A web interface for MySQL and M…   464       [OK]    

# 可选项 通过过滤来搜索
--filter=STARS=3000 #搜索镜像stars大于3000
Options:
  -f, --filter filter   Filter output based on conditions provided
      --format string   Pretty-print search using a Go template
      --limit int       Max number of search results (default 25)
      --no-trunc        Don't truncate output
```

**docker pull 下载镜像**

```shell
#下载镜像 docker pull 镜像名[:tag] 
ubuntu@VM-16-8-ubuntu:~$ docker pull mysql
Using default tag: latest #如果不写版本，默认是latest
latest: Pulling from library/mysql
72a69066d2fe: Pull complete  #分层下载 docker image的核心 联合文件系统
93619dbc5b36: Pull complete 
99da31dd6142: Pull complete 
626033c43d70: Pull complete 
37d5d7efb64e: Pull complete 
ac563158d721: Pull complete 
d2ba16033dad: Pull complete 
688ba7d5c01a: Pull complete 
00e060b6d11d: Pull complete 
1c04857f594f: Pull complete 
4d7cfa90e6ea: Pull complete 
e0431212d27d: Pull complete 
Digest: sha256:e9027fe4d91c0153429607251656806cc784e914937271037f7738bd5b8e7709 #签名
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest #真实地址

#等价于它
docker pull mysql
docker pull docker.io/library/mysql:lates

#指定版本下载
ubuntu@VM-16-8-ubuntu:~$ docker pull mysql:5.7
5.7: Pulling from library/mysql
72a69066d2fe: Already exists 
93619dbc5b36: Already exists 
99da31dd6142: Already exists 
626033c43d70: Already exists 
37d5d7efb64e: Already exists 
ac563158d721: Already exists 
d2ba16033dad: Already exists 
0ceb82207cd7: Pull complete 
37f2405cae96: Pull complete 
e2482e017e53: Pull complete 
70deed891d42: Pull complete 
Digest: sha256:f2ad209efe9c67104167fc609cca6973c8422939491c9345270175a300419f94
Status: Downloaded newer image for mysql:5.7
docker.io/library/mysql:5.7
```

**docker rmi 删除镜像**

```shell
ubuntu@VM-16-8-ubuntu:~$ docker rmi -f 镜像id #删除指定的容器
ubuntu@VM-16-8-ubuntu:~$ docker rmi -f 镜像id 容器id 容器id #删除多个容器
ubuntu@VM-16-8-ubuntu:~$ docker rmi -f $(docker images -aq) #删除全部的容器
```

## 容器命令

说明：有了镜像才能创建容器

```shell
docker pull centos
```

**新建容器并启动**

```shell
docker run [可选参数] image

# 参数说明
--name="Name"  容器名字 tomcat01 tomcat02，用来区分容器
-d 后台方式运行
-it 使用交互方式运行，进入容器查看内容
-p 指定容器的端口 -p 8080:8080
	-p ip:主机端口:容器端口 （常用）
	-p 容器端口
	容器端口
-P 随机指定端口

#测试 启动并进入容器
ubuntu@VM-16-8-ubuntu:~$ docker run -it centos /bin/bash 
[root@86bf01287367 /]# ls #查看容器内的centos
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@86bf01287367 /]# exit
exit
```

**列出所有运行中的容器**

```shell
docker ps 命令
Options:
  -a #列出当前正在运行的容器
  -n=？ #显示最近创建的容器
  -l, --latest          Show the latest created container (includes all states)
      --no-trunc        Don't truncate output
  -q, --quiet           Only display container IDs
  -s, --size            Display total file sizes
```

**退出容器**

```shell
exit #容器直接停止退出
Ctrl + p + q #容器不停止退出
```

**删除容器**

```shell
docker rm 容器id #删除指定容器，不能删除正在运行的容器，如果要强制删除rm -f
docker rm -f $(docker ps -aq) #删除所有容器
docker ps -a -q|xargs docker rm #删除所有的容器
```

**启动和停止容器操作**

```shell
docker start 容器id #启动容器
docker restart 容器id #重启容器
docker stop 容器id #停止当前正在运行的容器
docker kill 容器id #强制停止当前容器
```

## 常用其他命令

**后台启动容器**