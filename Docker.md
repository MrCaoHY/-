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

