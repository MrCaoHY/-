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

```shell
# 命令 docker run -d 镜像名
ubuntu@VM-16-8-ubuntu:~$ docker run -d centos

# 问题docker ps，发现 centos停止
# 常见问题：docker是容器使用后台运行，必须要有一个前台进程，docker发现没有应用，就会自动停止，例nginx容器启动后发现自己没有提供服务，就会立刻停止
```

**查看日志**

```shell
docker logs
Options:
      --details        Show extra details provided to logs
  -f, --follow         Follow log output
      --since string   Show logs since timestamp (e.g. 2013-01-02T13:23:37Z) or relative (e.g. 42m for 42 minutes)
  -n, --tail string    Number of lines to show from the end of the logs (default "all")
  -t, --timestamps     Show timestamps
      --until string   Show logs before a timestamp (e.g. 2013-01-02T13:23:37Z) or relative (e.g. 42m for 42 minutes)

docker logs -f -t --tail 容器，没有日志
#自己编写一个shell脚本
ubuntu@VM-16-8-ubuntu:~$ docker run -d centos /bin/sh -c "while true;do echo kuangsheng;sleep 1;done"
138688a0ccb0470f3ea03143e24a6bca6e89573d8d66f2fd982a666dce24e7f8
ubuntu@VM-16-8-ubuntu:~$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS        PORTS     NAMES
138688a0ccb0   centos    "/bin/sh -c 'while t…"   2 seconds ago   Up 1 second             recursing_blackburn
#显示日志
-tf #显示日志
--tail number #显示日志条数 
ubuntu@VM-16-8-ubuntu:~$ docker logs -tf --tail 10  138688a0ccb0
```

**查看容器中的进程信息** 

```shell
#命令 docker top 容器id
ubuntu@VM-16-8-ubuntu:~$ docker top 138688a0ccb0
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                645905              645885              0                   21:53               ?                   00:00:00            /bin/sh -c while true;do echo kuangsheng;sleep 1;done
root                647803              645905              0                   22:00               ?                   00:00:00            /usr/bin/coreutils --coreutils-prog-shebang=sleep /usr/bin/sleep 1
```

**查看镜像的元数据**

```shell
# 命令
docker inspect 容器id
ubuntu@VM-16-8-ubuntu:~$ docker inspect 138688a0ccb0
[
    {
        "Id": "138688a0ccb0470f3ea03143e24a6bca6e89573d8d66f2fd982a666dce24e7f8",
        "Created": "2022-03-07T13:53:46.307089567Z",
        "Path": "/bin/sh",
        "Args": [
            "-c",
            "while true;do echo kuangsheng;sleep 1;done"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 645905,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2022-03-07T13:53:46.686073644Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:5d0da3dc976460b72c77d94c8a1ad043720b0416bfc16c52c45d4847e53fadb6",
        "ResolvConfPath": "/var/lib/docker/containers/138688a0ccb0470f3ea03143e24a6bca6e89573d8d66f2fd982a666dce24e7f8/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/138688a0ccb0470f3ea03143e24a6bca6e89573d8d66f2fd982a666dce24e7f8/hostname",
        "HostsPath": "/var/lib/docker/containers/138688a0ccb0470f3ea03143e24a6bca6e89573d8d66f2fd982a666dce24e7f8/hosts",
        "LogPath": "/var/lib/docker/containers/138688a0ccb0470f3ea03143e24a6bca6e89573d8d66f2fd982a666dce24e7f8/138688a0ccb0470f3ea03143e24a6bca6e89573d8d66f2fd982a666dce24e7f8-json.log",
        "Name": "/recursing_blackburn",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "docker-default",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "CgroupnsMode": "host",
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "ConsoleSize": [
                0,
                0
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "KernelMemory": 0,
            "KernelMemoryTCP": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/dca71537ec0efcc468a03f3e3b959c7a64d77123b63d6d74c94e19636863c9b1-init/diff:/var/lib/docker/overlay2/c23ad7d1bd6160b3010dd897fc946b1e8838e8029f7b56b2210f93ace06e3b73/diff",
                "MergedDir": "/var/lib/docker/overlay2/dca71537ec0efcc468a03f3e3b959c7a64d77123b63d6d74c94e19636863c9b1/merged",
                "UpperDir": "/var/lib/docker/overlay2/dca71537ec0efcc468a03f3e3b959c7a64d77123b63d6d74c94e19636863c9b1/diff",
                "WorkDir": "/var/lib/docker/overlay2/dca71537ec0efcc468a03f3e3b959c7a64d77123b63d6d74c94e19636863c9b1/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "138688a0ccb0",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/sh",
                "-c",
                "while true;do echo kuangsheng;sleep 1;done"
            ],
            "Image": "centos",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "org.label-schema.build-date": "20210915",
                "org.label-schema.license": "GPLv2",
                "org.label-schema.name": "CentOS Base Image",
                "org.label-schema.schema-version": "1.0",
                "org.label-schema.vendor": "CentOS"
            }
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "be796ec0a125ae2b2443812b0fca6e15239f612dbfcaa31937bbae0aaefcee97",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/be796ec0a125",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "bbae773252890099a9da8c2356730a39fa44ba40bd22aa4609353b0af6b2095b",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "cf4488d3f41f081cdc50a063bf7069bac110edae6555ee6d98a2e69488bdc66f",
                    "EndpointID": "bbae773252890099a9da8c2356730a39fa44ba40bd22aa4609353b0af6b2095b",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
        }
    }
]
```

**进入当前正在运行的容器**

```shell
#我们的容器使用后台方式运行的，需要进入容器，修改配置
# 命令
docker exec -it 容器id bashshell
# 测试
ubuntu@VM-16-8-ubuntu:~$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS     NAMES
138688a0ccb0   centos    "/bin/sh -c 'while t…"   20 minutes ago   Up 20 minutes             recursing_blackburn
ubuntu@VM-16-8-ubuntu:~$ docker exec -it 138688a0ccb0 bashshell
OCI runtime exec failed: exec failed: container_linux.go:380: starting container process caused: exec: "bashshell": executable file not found in $PATH: unknown
ubuntu@VM-16-8-ubuntu:~$ docker exec -it 138688a0ccb0 /bin/bash
[root@138688a0ccb0 /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@138688a0ccb0 /]# ps -ef
UID          PID    PPID  C STIME TTY          TIME CMD
root           1       0  0 13:53 ?        00:00:00 /bin/sh -c while true;do echo kuangsheng;sleep 1;done
root        1276       0  0 14:14 pts/0    00:00:00 /bin/bash
root        1306       1  0 14:15 ?        00:00:00 /usr/bin/coreutils --coreutils-prog-shebang=sleep /usr/bin/sleep 1
root        1307    1276  0 14:15 pts/0    00:00:00 ps -ef

# 方式二
docker attach 容器id
# 测试
[root@138688a0ccb0 /]# docker attach 138688a0ccb0
正在执行当前代码...

#docker exec # 进入容器后开启一个新的终端，可以在里面操作(常用)
#docker attach # 进入容器正在执行的终端，不会启动新的进程
```

**从容器内拷贝文件到主机**

```shell
docker cp 容器id：容器内路径 目的主机路径
#查看当前主机目录下
ubuntu@VM-16-8-ubuntu:/home$ ls
chy.java  lighthouse  ubuntu
ubuntu@VM-16-8-ubuntu:/home$ docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED         STATUS         PORTS     NAMES
1bb6ee136d05   centos    "/bin/bash"   5 minutes ago   Up 5 minutes             cranky_meitner
#进入docker主机内部
ubuntu@VM-16-8-ubuntu:/home$ docker attach 1bb6ee136d05 
[root@1bb6ee136d05 home]# touch test.java
[root@1bb6ee136d05 home]# ls
test.java
[root@1bb6ee136d05 home]# exit
exit
buntu@VM-16-8-ubuntu:/home$ docker ps -a
CONTAINER ID   IMAGE     COMMAND       CREATED         STATUS                       PORTS     NAMES
1bb6ee136d05   centos    "/bin/bash"   7 minutes ago   Exited (0) 12 seconds ago              cranky_meitner
b6f4db898e4e   centos    "/bin/bash"   8 minutes ago   Exited (127) 7 minutes ago             strange_easley
#将文件拷贝至主机上 
ubuntu@VM-16-8-ubuntu:/home$ sudo docker cp 1bb6ee136d05:/home/test.java /home
ubuntu@VM-16-8-ubuntu:/home$ ls
chy.java  lighthouse  test.java  ubuntu
# 拷贝只是一个手动过程，蔚来使用-V卷的技术，可以实现
```

## 小结

![v2-820aee2a33654099d87cdd2b7a1ce741_r.jpg](https://s2.loli.net/2022/03/07/PCFwS6o1AsJVzme.jpg)

[Docker 命令大全_w3cschool](https://www.w3cschool.cn/docker/docker-command-manual.html)

## 练习

> Docker安装Nginx

```shell
#搜索镜像 search 建议去docker hub上，可以看到详细信息

#下载镜像 pull
ubuntu@VM-16-8-ubuntu:~$ docker pull nginx
Using default tag: latest
latest: Pulling from library/nginx
a2abf6c4d29d: Pull complete 
a9edb18cadd1: Pull complete 
589b7251471a: Pull complete 
186b1aaa4aa6: Pull complete 
b4df32aa5a72: Pull complete 
a0bcbecc962e: Pull complete 
Digest: sha256:0d17b565c37bcbd895e9d92315a05c1c3c9a29f762b011a10c54a66cd53c9b31
Status: Downloaded newer image for nginx:latest
docker.io/library/nginx:latest
#运行测试 -d 后台运行 --name命名 -p宿主机端口：容器内端口
ubuntu@VM-16-8-ubuntu:~$ docker run -d --name nginx01 -p 3344:80 nginx
1f4249a82e3cc7baebafd3a3e5ceee037be49ba5f6a352b9a0d10c3df1f16867

ubuntu@VM-16-8-ubuntu:~$ curl localhost:3344
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>

#进入容器
ubuntu@VM-16-8-ubuntu:~$ docker exec -it nginx01 /bin/bash
root@1f4249a82e3c:/# whereis nginx
nginx: /usr/sbin/nginx /usr/lib/nginx /etc/nginx /usr/share/nginx
root@1f4249a82e3c:/# cd /etc/nginx/
root@1f4249a82e3c:/etc/nginx# ls
conf.d  fastcgi_params  mime.types  modules  nginx.conf  scgi_params  uwsgi_params
```

端口暴露的概念

![查看源图像](https://tse3-mm.cn.bing.net/th/id/OIP-C.3bngYwuyytT4ns2svnx4SgHaFz?pid=ImgDet&rs=1)

思考:每次改动nginx配置文件，都需要进入容器内部，十分麻烦，在容器外提供一个映射路径，在容器外部修改文件，容器内部就可以自动修改。



> 部署tomcat

```shell
#官方的使用
$ docker run -it --rm tomcat:9.0
#我们之前启动的是后台，停止了容器之后可以查到， docker run -it --rm一般用来测试用完即删

#下载再启动
docker pull tomcat:9.0

#启动运行
docker run -d -p 3355:8080 --name tomcat01 tomcat
#测试tomcat访问是否正常
#进入容器 发现问题：1、linux命令少了 2.没有webapps 镜像的原因，默认是最小的镜像，所有不必要的都删除掉，保存最小可运行的环境
ubuntu@VM-16-8-ubuntu:~$ docker exec -it tomcat01 /bin/bash
root@892d34b2ee10:/usr/local/tomcat# ll
bash: ll: command not found
root@892d34b2ee10:/usr/local/tomcat# ls
BUILDING.txt     LICENSE  README.md      RUNNING.txt  conf  logs            temp     webapps.dist
CONTRIBUTING.md  NOTICE   RELEASE-NOTES  bin          lib   native-jni-lib  webapps  work
root@892d34b2ee10:/usr/local/tomcat# cd webapps
root@892d34b2ee10:/usr/local/tomcat/webapps# ls
```

思考：部署项目，每次都要进入容器十分麻烦，可以在容器外部提供一个映射路径，webapps，我们在外部放置项目，就自动同步到内部就好了

docker容器 tomcat + 网站 mysql

> 部署es+kibana

```shell
# es 暴露的端口很多
# es 十分耗内存
# es 的数据一般需要放置到安全目录！挂载
# --net somenetwork ? 网络配置
```

## 可视化

* portainer(先用这个)

```shell
docker run -d -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer
cd /usr/libexec/docker/
```

* Rancher(CI/CD再用)

**什么是portainer？**

Docker图形化界面管理工具！提供一个后台面板供我们操作！

平时不会使用

# Docker镜像加载原理

## 镜像是什么

镜像是一种轻量级、可执行的独立软件包，用来打包软件运行环境和基于运行环境开发的软件，它包含运行某个软件所需的所有内容，包括代码、运行时、库、环境变量和配置文件

所有的应用，直接打包docker镜像，就可以直接跑起来

如何得到镜像

* 从远程仓库下载
* 拷贝现有的
* 自己制作一个镜像DockerFile

## Docker镜像加载原理

> [Docker镜像原理 - 掘金 (juejin.cn)](https://juejin.cn/post/6996310038419603463)

> UnionFS(联合文件系统)

UnionFS（联合文件系统）：Union文件系统（UnionFS）是一种分层、轻量级并且高性能的文件系统，它支持对文件系统的修改作为一次提交来一层层的叠加，同时可以将不同目录挂载到同一个虚拟文件系统下(unite several directories into a single virtual filesystem)。Union 文件系统是 Docker 镜像的基础。镜像可以通过分层来进行继承，基于基础镜像（没有父镜像），可以制作各种具体的应用镜像。 **特性**：一次同时加载多个文件系统，但从外面看起来，只能看到一个文件系统，联合加载会把各层文件系统叠加起来，这样最终的文件系统会包含所有底层的文件和目录

下载docker镜像时一层一层的其实就是联合文件系统的体现

> Docker镜像加载原理

docker的镜像实际上由一层一层的文件系统组成，这种层级的文件系统UnionFS。 bootfs(boot file system)主要包含bootloader和kernel, bootloader主要是引导加载kernel, Linux刚启动时会加载bootfs文件系统，在Docker镜像的最底层是bootfs。这一层与我们典型的Linux/Unix系统是一样的，包含boot加载器和内核。当boot加载完成之后整个内核就都在内存中了，此时内存的使用权已由bootfs转交给内核，此时系统也会卸载bootfs。

rootfs (root file system) ，在bootfs之上。包含的就是典型 Linux 系统中的 /dev, /proc, /bin, /etc 等标准目录和文件。rootfs就是各种不同的操作系统发行版，比如Ubuntu，Centos等等。 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/475fbb6fbeb7478da1bc38e94ef1b567~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

平时我们安装虚拟机的CentOs都是好几个G，为什么docker才几百M

对于一个精简的OS，rootfs可以很小，只需要包括最基本的命令、工具和程序库就可以了，因为底层直接用Host的kernel，自己只需要提供 rootfs 就行了。由此可见对于不同的linux发行版, bootfs基本是一致的, rootfs会有差别, 因此不同的发行版可以公用bootfs。

## 分层理解

下载docker镜像时一层一层的其实就是分层最直观的体现（已经下载过得不会重复下载）

```shell
$ docker pull redis
Using default tag: latest
latest: Pulling from library/redis
33847f680f63: Already exists 
26a746039521: Pull complete
18d87da94363: Pull complete
5e118a708802: Pull complete
ecf0dbe7c357: Pull complete
46f280ba52da: Pull complete
Digest: sha256:cd0c68c5479f2db4b9e2c5fbfdb7a8acb77625322dd5b474578515422d3ddb59
Status: Downloaded newer image for redis:latest
docker.io/library/redis:latest
```

我们还可以通过之前文章中提到的inspect命令

```shell
docker image inspect redis:latest
```

可以看到镜像的具体分层信息，有6层对应上面镜像下载时候的6层

```shell
"RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:814bff7343242acfd20a2c841e041dd57c50f0cf844d4abd2329f78b992197f4",
                "sha256:dd1ebb1f5319785e34838c7332a71e5255bda9ccf61d2a0bf3bff3d2c3f4cdb4",
                "sha256:11f99184504048b93dc2bdabf1999d6bc7d9d9ded54d15a5f09e36d8c571c32d",
                "sha256:e461360755916af80821289b1cbc503692cf63e4e93f09b35784d9f7a819f7f2",
                "sha256:45f6df6342536d948b07e9df6ad231bf17a73e5861a84fc3c9ee8a59f73d0f9f",
                "sha256:262de04acb7e0165281132c876c0636c358963aa3e0b99e7fbeb8aba08c06935"
            ]
        },

```

**分层的好处**：

最大的一个好处就是 - 共享资源

比如：有多个镜像都从相同的 base 镜像构建而来，那么 Docker Host 只需在磁盘上保存一份 base 镜像；同时内存中也只需加载一份 base 镜像，就可以为所有容器服务了。而且镜像的每一层都可以被共享。

这时可能就有人会问了：如果多个容器共享一份基础镜像，当某个容器修改了基础镜像的内容，比如 /etc 下的文件，这时其他容器的 /etc 是否也会被修改？ 答案：不会！因为修改会被限制在单个容器内。

这就是我们接下来要学习的容器 Copy-on-Write 特性

## 容器的可写层

当容器启动时，一个新的可写层被加载到镜像的顶部。 这一层通常被称作“容器层”，“容器层”之下的都叫“镜像层”。所有操作都是针对容器层的，只有容器层是可写的，容器层下面的所有镜像层都是只读的。

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3fa08882aa0e4c71a4778d9647935317~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

## commit镜像

```shell
docker commit  #提交容器成为一个新的副本
```

我们下面通过一个例子说明下

我这边之前下载过tomcat的镜像

```shell
$ docker images
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
tomcat       latest    710ec5c56683   7 days ago     668MB
redis        latest    aa4d65e670d6   3 weeks ago    105MB
mysql        latest    c60d96bd2b77   3 weeks ago    514MB
centos       centos7   8652b9f0cb4c   9 months ago   204MB
复制代码
```

将tomcat启动起来

```shell
# 镜像运行起来，并且将端口进行映射到宿主机端口
docker run  -it -p 8080:8080 tomcat
```

在另外一个创建通过命令查看下当前运行的容器,tomcat正在运行

```shell
$ docker ps
CONTAINER ID   IMAGE            COMMAND             CREATED          STATUS          PORTS                                       NAMES
8b294cd7074e   tomcat           "catalina.sh run"   29 seconds ago   Up 28 seconds   0.0.0.0:8080->8080/tcp, :::8080->8080/tcp   zealous_cohen
f42ae22e4b72   centos:centos7   "/bin/bash"         3 weeks ago      Up 46 hours                                                 centos-test
```

然后我们进入tomcat这个容器看下哈

```shell
docker exec -it 8b294cd7074e /bin/bash
复制代码
$ docker exec -it 8b294cd7074e /bin/bash
root@8b294cd7074e:/usr/local/tomcat# ls
BUILDING.txt  CONTRIBUTING.md  LICENSE	NOTICE	README.md  RELEASE-NOTES  RUNNING.txt  bin  conf  lib  logs  native-jni-lib  temp  webapps  webapps.dist  work
```

然后再进入webapp（项目文件位置）中看下,如下可以看到webapp中是空的

```shell
root@8b294cd7074e:/usr/local/tomcat# cd webapps
root@8b294cd7074e:/usr/local/tomcat/webapps# ls
root@8b294cd7074e:/usr/local/tomcat/webapps#
```

然后我们访问下tomcat，404说明tomcat是正常启动了，只是没有找到对应的页面，这也容易理解应为wabapp中是空的

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b5540c1e1f134270813532163a4611c8~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

那么我们尝试自己做一个webapp中有内容的镜像

我们将webapps.dist中的文件copy到wabapp中

```shell
root@8b294cd7074e:/usr/local/tomcat# cp -r webapps.dist/* webapps
root@8b294cd7074e:/usr/local/tomcat# cd webapps
root@8b294cd7074e:/usr/local/tomcat/webapps# ls
ROOT  docs  examples  host-manager  manager
```

修改之后发现tomcat可以正常访问了

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f8b7088046d348d5b2933cbdddb14903~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

```shell
$ docker ps
CONTAINER ID   IMAGE            COMMAND             CREATED          STATUS          PORTS                                       NAMES
8b294cd7074e   tomcat           "catalina.sh run"   27 minutes ago   Up 27 minutes   0.0.0.0:8080->8080/tcp, :::8080->8080/tcp   zealous_cohen
```

现在这个tomcat我们做了简单的修改，我觉得我的这个tomcat比官方的好一点点，我把我这个镜像提交下

```shell
$ docker commit -a="cb" -m "add init file" 8b294cd7074e newtomcat:1.0
sha256:44cf4d44be664d9704a3fc38ddef1f03fa7f113ad83f4049cced322a14dc216b
```

通过docker images可以看到有新镜像就创建好了，仔细观察可以发现我们提交的额newtomcat比官方的tomcat要大一点

```shell
$ docker images
REPOSITORY   TAG       IMAGE ID       CREATED          SIZE
newtomcat    1.0       44cf4d44be66   46 seconds ago   673MB
tomcat       latest    710ec5c56683   7 days ago       668MB
redis        latest    aa4d65e670d6   3 weeks ago      105MB
mysql        latest    c60d96bd2b77   3 weeks ago      514MB
centos       centos7   8652b9f0cb4c   9 months ago     204MB
```

后面就可以直接使用自己的镜像了，也可以发布出去给别人使用，这个后面在说

通过这个例子可以回顾下上面的分层思想，newtomcat是我们在之前容器层做了修改后生成的镜像，这个镜像和虚拟机的镜像快照差不多

关于docker的镜像就说到这里，到现在docker算是简单的入了个门，后面会继续学习docker相关的内容，大家一起加油！

# 容器数据卷

## 什么是容器数据卷

**Docker 理念回顾**

将应用和环境打包成一个镜像

如果数据都在容器中，那么容器删除，数据就会丢失！需求：数据可以持久化

容器中间可以有一个数据共享的技术，Docker容器产生的数据可以同步到本地，这就是容器技术，目录的挂载 ，将容器的目录挂载到linux上

**总结一句话：容器的持久化和同步操作！容器间也是可以数据共享的**

## 使用数据卷

> 方式一：直接使用命令挂载 -v

```shell
docker run -it -v 主机目录:容器目录
# 测试
ubuntu@VM-16-8-ubuntu:/home$ docker run it -v /home/ceshi/:/home centos /bin/bash
# 容器启动之后通过docker inspect 容器id
```

<img src="https://s2.loli.net/2022/03/11/PdC9j2OYDqtTVHe.jpg" alt="屏幕截图 2022-03-11 154258.jpg" style="zoom:67%;" />

## 实战：安装MySQL

思考:MySQL的数据持久化问题

```shell
#获取镜像
ubuntu@VM-16-8-ubuntu:~$ docker pull mysql:5.7
#运行容器，需要做数据卷挂载 安装mysq，需要配置密码
# 官方配置密码$ docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag

# 启动
ubuntu@VM-16-8-ubuntu:~$ docker run -d -p 3310:3306 -v /home/mysql/conf:/etc/mysql/conf.d -v /home/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 --name mysql01 mysql:5.7
#启动成功之后，连接测试一下
#容器删除后，挂载到本地的数据依然不会删除，实现容器数据持久化
```

## 具名挂载和匿名挂载

```shell
-v 容器内路径 #匿名挂载
docker run -d -P --name nginx01 -v etc/nginx
#查看所有volume的情况
ubuntu@VM-16-8-ubuntu:~$ docker volume ls
DRIVER    VOLUME NAME
local     93f0c8fac224b2c6bc686e910858019a0ad6380c9e070826b11b248c6cb1a613
# 这里发现的这种都是匿名挂载，只有容器内部路径没有容器外的
-v 卷名：容器内路径 #具名挂载
ubuntu@VM-16-8-ubuntu:/home$  docker run -d -P --name nginx02 -v juming-nginx:/etc/nginx nginx
682c3db14b301dd1c5b8a16073b07f5af27238ab7d24de2719efc015c4d54a66
ubuntu@VM-16-8-ubuntu:/home$ docker volume ls
DRIVER    VOLUME NAME
local     93f0c8fac224b2c6bc686e910858019a0ad6380c9e070826b11b248c6cb1a613
local     juming-nginx
#查看此卷
ubuntu@VM-16-8-ubuntu:/home$ docker volume inspect juming-nginx
[
    {
        "CreatedAt": "2022-03-11T22:04:26+08:00",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/juming-nginx/_data",
        "Name": "juming-nginx",
        "Options": null,
        "Scope": "local"
    }
]

所有docker内容的卷，没有指定目录的情况下都是在：/var/lib/docker/volumes/XXX
-v /宿主内路径:容器内路径 #指定路径挂载
```

我们通过具名挂载的方式可以很方便的找到我们的卷，大多数情况在使用的'具名挂载'

拓展：

```shell
# 通过 -v 容器内路径：ro rw 改变读写权限
ro readonly #只读，这个路径只能通过宿主机来操作，容器内无法操作
rw readwrite #可读可写
# 一旦设定了这个,容器辉挂载出来的内容就有限定了
ubuntu@VM-16-8-ubuntu:/home$  docker run -d -P --name nginx02 -v juming-nginx:/etc/nginx:ro nginx
ubuntu@VM-16-8-ubuntu:/home$  docker run -d -P --name nginx02 -v juming-nginx:/etc/nginx:rw nginx
```

## 初识DockerFile

DockerFile就是用来构建docker镜像的构建文件

通过这个脚本可以生成镜像，镜像是一层一层的，脚本一个个的命令，每个命令都是一层

## 数据卷容器

多个mysql同步数据

![查看源图像](https://tse1-mm.cn.bing.net/th/id/R-C.76a267ed0f07384e607e0e9ec483ec37?rik=RJ32lIP7Bsl2sw&riu=http%3a%2f%2fimage1.bubuko.com%2finfo%2f202101%2f20210127224242418539.png&ehk=NHcC84vpcez%2fhPQ3yXfQDG%2fzxwT8txJiWEVWSWS%2bP8k%3d&risl=&pid=ImgRaw&r=0)





# DockerFile

dockerfile是用来构建docker镜像的文件，命令参数脚本

构建步骤：

1. 编写一个dockerfile文件
2. docker builder构建成为一个镜像
3. docker run运行镜像
4. docker push发布镜像（dockerhub、阿里云镜像仓库）
5. 官方做法

```shell
FROM scratch
ADD centos-7-x86_64-docker.tar.xz /

LABEL \
    org.label-schema.schema-version="1.0" \
    org.label-schema.name="CentOS Base Image" \
    org.label-schema.vendor="CentOS" \
    org.label-schema.license="GPLv2" \
    org.label-schema.build-date="20201113" \
    org.opencontainers.image.title="CentOS Base Image" \
    org.opencontainers.image.vendor="CentOS" \
    org.opencontainers.image.licenses="GPL-2.0-only" \
    org.opencontainers.image.created="2020-11-13 00:00:00+00:00"

CMD ["/bin/bash"]
```

很多官方镜像都是基础包，很多功能都没有，我们通常会自己搭建自己的镜像！



## DockerFile的构建过程

基础知识：

1. 每个保留关键字（指令）都必须是大写字母
2. 执行从上至下顺序执行
3. #表示注释
4. 每个指令都会创建提交一个新的镜像层，并提交

<img src="https://img-blog.csdnimg.cn/20200826113041394.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjYyMjY2,size_16,color_FFFFFF,t_70" alt="查看源图像" style="zoom: 50%;" />

docker是面向开发的，我们以后要发布项目，做镜像，就需要编写dockerfile文件

DockerFile:构建文件，定义了一切的步骤，源代码

Dockeriamges:通过DockerFile构建生成的镜像，最终发布和运行的产品

Docker容器：容器就是镜像运行起来提供服务的

## DockerFile的指令

```shell
RUN #基础镜像，一般从这里开始构建
MAINTAINER #镜像是谁写的，姓名+邮箱
RUN #镜像构建的时候需要的命令
ADD #步骤：tomcat镜像，这个tomcat压缩包！添加内容
WORKIR #镜像的工作目录
VOLUME #挂载的目录
EXPOST #保留端口配置
CMD #指定这个容器启动时要运行的命令，只有最后一个会生效，可被替代
ENTRYPOINT #指定这个容器启动时要运行的命令，可追加命令
ONBUILD #当构建一个被继承DockerFile这个时候就会运行，触发指令
COPY #类似ADD，将文件拷贝到镜像中
ENV #构建的时候设置环境变量
```



<img src="https://tse1-mm.cn.bing.net/th/id/R-C.7e554c8e9afda05e73075785c958c3b1?rik=pVLDhGgoiug4pQ&riu=http%3a%2f%2fstatic.gitlib.com%2fblog%2f2017%2f10%2f27%2fdocker2.jpg&ehk=rX%2fmx8o28le1zNp8NyBbI9liiVAlptu%2btk4SJQp84kw%3d&risl=&pid=ImgRaw&r=0&sres=1&sresct=1" alt="查看源图像" style="zoom: 50%;" />

## 实战测试

 Docker Hub中99%的镜像都是从这个基础镜像过来的FORM scratch，然后配置需要的软件和配置来进行构建                       

> 创建一个自己的centos

 ```shell
 # 1.编写dockerfile文件
 root@VM-16-8-ubuntu:/home/dockerfile# cat mydocker-centos 
 FROM centos
 MAINTAINER chy<caohaiyang666888@outlook.com>
 
 ENV MYPATH /usr/local
 
 WORKDIR $MYPATH
 
 RUN yum -y install vim
 RUN yum -y install net-tools
 
 EXPOSE 80
 
 CMD echo $MYPATH
 CMD echo "----end----"
 CMD /bin/bash
 # 2.通过这个文件构造镜像
 # 命令 docker builder -f dockerfile文件路径 -t 镜像名:[tag]
 root@VM-16-8-ubuntu:/home/dockerfile# docker build -f mydocker-centos -t mycentos:0.1 .
 # 3.测试运行
 ```

 查看本地镜像变更历史

 ```shell
root@VM-16-8-ubuntu:~# docker history 5d0da3dc9764
IMAGE          CREATED        CREATED BY                                      SIZE      COMMENT
5d0da3dc9764   5 months ago   /bin/sh -c #(nop)  CMD ["/bin/bash"]            0B        
<missing>      5 months ago   /bin/sh -c #(nop)  LABEL org.label-schema.sc…   0B        
<missing>      5 months ago   /bin/sh -c #(nop) ADD file:805cb5e15fb6e0bb0…   231MB     
root@VM-16-8-ubuntu:~# docker history 3218b38490ce
IMAGE          CREATED        CREATED BY                                      SIZE      COMMENT
3218b38490ce   2 months ago   /bin/sh -c #(nop)  CMD ["mysqld"]               0B        
<missing>      2 months ago   /bin/sh -c #(nop)  EXPOSE 3306 33060            0B        
<missing>      2 months ago   /bin/sh -c #(nop)  ENTRYPOINT ["docker-entry…   0B        
<missing>      2 months ago   /bin/sh -c ln -s usr/local/bin/docker-entryp…   34B       
<missing>      2 months ago   /bin/sh -c #(nop) COPY file:345a22fe55d3e678…   14.5kB    
<missing>      2 months ago   /bin/sh -c #(nop) COPY dir:2e040acc386ebd23b…   1.12kB    
<missing>      2 months ago   /bin/sh -c #(nop)  VOLUME [/var/lib/mysql]      0B        
<missing>      2 months ago   /bin/sh -c {   echo mysql-community-server m…   380MB     
<missing>      2 months ago   /bin/sh -c echo 'deb http://repo.mysql.com/a…   55B       
<missing>      2 months ago   /bin/sh -c #(nop)  ENV MYSQL_VERSION=8.0.27-…   0B        
<missing>      2 months ago   /bin/sh -c #(nop)  ENV MYSQL_MAJOR=8.0          0B        
<missing>      2 months ago   /bin/sh -c set -ex;  key='A4A9406876FCBD3C45…   1.84kB    
<missing>      2 months ago   /bin/sh -c apt-get update && apt-get install…   52.2MB    
<missing>      2 months ago   /bin/sh -c mkdir /docker-entrypoint-initdb.d    0B        
<missing>      2 months ago   /bin/sh -c set -eux;  savedAptMark="$(apt-ma…   4.17MB    
<missing>      2 months ago   /bin/sh -c #(nop)  ENV GOSU_VERSION=1.12        0B        
<missing>      2 months ago   /bin/sh -c apt-get update && apt-get install…   9.34MB    
<missing>      2 months ago   /bin/sh -c groupadd -r mysql && useradd -r -…   329kB     
<missing>      2 months ago   /bin/sh -c #(nop)  CMD ["bash"]                 0B        
<missing>      2 months ago   /bin/sh -c #(nop) ADD file:bd5c9e0e0145fe33b…   69.3MB  
 ```

> CMD和ENTRYPOINT区别

CMD #指定这个容器启动时要运行的命令，只有最后一个会生效，可被替代
ENTRYPOINT #指定这个容器启动时要运行的命令，可追加命令

## 实战：tomcat镜像

1. 准备镜像文件tomcat压缩包，jdk的压缩包
2. 编写dockerfile文件     

## 发布镜像

## Docker流程小结

<img src="https://s2.loli.net/2022/03/13/snafo5wLNMJt6Wx.png" alt="3f0c8185c0b937e98f7a690779dc305e.png" style="zoom:50%;" />



# Docker网络

# 企业实战

## DockerCompose

## DockerSwam

## CI/CD Jenkins