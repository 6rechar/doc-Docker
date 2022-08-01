# Docker

## 概述

基于Linux下运行环境学习Docker



## Docker入门

Docker为什么会出现？

产品：开发 -- 上线	两套环境！（应用环境，应用配置）

```
背景：

产品上线的前一周（研发）小黑将打包好的程序交付给（运维）小红测试

小红：小黑啊，你这又给我写bug呢。。。这程序运行不起来！

小黑：不可能，绝对不可能！在我本地就可以运行起来啊！

问题：在小黑的电脑上可以运行！版本更新，导致服务不可用！对于运维来说，考验就比较大了。

经过一段时间的问题排查，小黑发现小红电脑的运行环境和自己本地不同。

小红：环境配置是十分麻烦的，每一台机器都要部署环境（.net运行时环境 Redis，mysql……）这样时间成本就非常大。

小红：发布一个项目,项目能不能都带上环境安装打包？之前在服务器配置一个应用的环境Redis、MySql、.net 6……都超级麻烦，不能跨平台。
```

Docker 成功解决了以上问题。

传统：程序打包，运维安装本地环境。

现在：开发打包部署上线，一站式完成。



![img](.\Assests\icon.jpg)

Docker思想就来自于集装箱！

jdk —— 多个应用（端口冲突）—— 原来都是交叉的！

隔离 —— Docker核心思想！打包装箱！每个箱子都是相互隔离的。

Docker通过隔离机制，可以将服务器利用到极致！



### 虚拟化技术和容器化技术对比

#### 虚拟化技术的缺点

- 资源占用十分多
- 冗余步骤多
- 启动慢

![在这里插入图片描述](D:\Learning\技术栈\Docker\Assests\dockers1.jpg)

#### 容器化技术

![在这里插入图片描述](D:\Learning\技术栈\Docker\Assests\docker2.jpg)

- **比较Docker和虚拟化技术的不同**

  - 传统虚拟机， 虚拟出一条硬件，运行一个完整的操作系统，然后在这个系统上安装和运行软件。

  - 容器内的应用直接运行在宿主机的内部，容器是没有自己的内核的，也没有虚拟硬件，所以轻便。

  - 每个容器间是相互隔离的，每个容器内都有一个属于自己的文件系统，互不影响。

- **应用更快速的交互和部署**

  - 传统：一堆帮助文档，安装程序

  - Docker： 打包镜像发布测试，一键运行

- **更便捷的升级和扩缩容**

- **更简的系统运维**

- **更高效的计算资源利用**

#### DevOps（开发、运维）

- **应用更快速的交付和部署**

  - 传统：一堆帮助文档、安装程序。

  - Docker：打包镜像发布测试，一键运行。

- **更便捷的升级和扩展容**

  - 使用了Docker之后，我们部署应用就和搭积木一样！

  - 项目打包一个镜像，扩展 - 服务器A！服务器B！

- **更简单的系统运维**
  - 在容器化之后，开发、测试环境都是高度一致的。

- **更搞笑的计算资源利用**
  - Docker是内核级别的虚拟化，可以再一个物理机上可以运行很多的容器实例！服务器的性能可以被压榨到极致。

### 名词解释

- **镜像（image）**
  - Docker镜像就好比是一个模板，可以通过这个模板来创建容器服务，tomcat镜像 ===> run ===> tomcat01容器， 通过这个镜像可以创建多个容器（最终服务运行或者项目运行就是在容器中的）。

- **容器（container）**
  - Docker利用容器技术，独立运行一个或者一组应用， 通过镜像来创建的启动，停止，删除，基本命令！就目前可以把这个容器理解为一个建议的linux系统。

- **仓库（repository）**
  - 存放镜像的地方
  - Docker Hub（默认是国外的）。
  - 阿里云,,,都有容器服务（配置镜像加速！）

### 阿里云镜像加速

登录阿里云服务器，找到`容器镜像服务`

设置Registry登录密码

找到镜像加速器

配置使用

```linux
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://pi9dpp60.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

[具体查看参考文献](https://blog.csdn.net/weixin_43830765/article/details/123519840)



### Docker安装

使用 Docker 仓库进行安装
在新主机上首次安装 Docker Engine-Community 之前，需要设置 Docker 仓库。之后，您可以从仓库安装和更新 Docker。

设置仓库

安装所需的软件包。yum-utils 提供了 yum-config-manager ，并且 device mapper 存储驱动程序需要 device-mapper-persistent-data 和 lvm2。

帮助文档（以下命令需要在root权限下运行）：

```
1、卸载旧版本
yum remove docker \
          docker-client \
          docker-client-latest \
          docker-common \
          docker-latest \
          docker-latest-logrotate \
          docker-logrotate \
          docker-engine
2、需要的安装包
yum install -y yum-utils

3、设置镜像仓库
yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo		# 默认是国外的
    
yum-config-manager \
    --add-repo \
    http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo		# 阿里云（推荐）
    
yum-config-manager \
    --add-repo \
    https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/centos/docker-ce.repo		# 清华大学源
    
3、更新yum软件包的索引
yum makecache fast

4、安装docker相关的内容	（docker-ce	社区版;	ee	企业版;）
yum install docker-ce docker-ce-cli containerd.io

5、启动docker
systemctl start docker
```

**也可以指定版本下载**

```Linux
1、列出并排序您存储库中可用的版本。此示例按版本号（从高到低）对结果进行排序。
$ yum list docker-ce --showduplicates | sort -r
docker-ce.x86_64  3:18.09.1-3.el7                     docker-ce-stable
docker-ce.x86_64  3:18.09.0-3.el7                     docker-ce-stable
docker-ce.x86_64  18.06.1.ce-3.el7                    docker-ce-stable
docker-ce.x86_64  18.06.0.ce-3.el7                    docker-ce-stable

2、通过其完整的软件包名称安装特定版本，该软件包名称是软件包名称（docker-ce）加上版本字符串（第二列），从第一个冒号（:）一直到第一个连字符，并用连字符（-）分隔。例如：docker-ce-18.09.1。
$ sudo yum install docker-ce-<VERSION_STRING> docker-ce-cli-<VERSION_STRING> containerd.io
启动 Docker。

$ sudo systemctl start docker
通过运行 hello-world 映像来验证是否正确安装了 Docker Engine-Community 。

$ sudo docker run hello-world
```



### 底层原理

![image-20220728094217535](D:\Learning\技术栈\Docker\Assests\docker5.png)















### Docker镜像讲解

**镜像是什么**

镜像是一种轻量级、可执行的独立的软件包，用来打包软件运行环境和基于运行环境开发的软件，它包含运行某个软件所需要的所有内容，包括代码、运行时环境、库、环境变量和配置文件。

所有的应用，直接打包docker镜像，就可以直接跑起来。

如何得到镜像：

- 从远程仓库下载
- 朋友那获取
- 自己制作一个镜像DockerFile









Docker镜像加载原理

联合文件系统：UnionFS

联合文件系统（UnionFS）是一种分层、轻量级并且高性能的文件系统，它支持对文件系统的修改作为一次提交来一层层的叠加，同时可以将不同目录挂载到同一个虚拟文件系统下。
联合文件系统是 Docker 镜像的基础。镜像可以通过分层来进行继承，基于基础镜像（没有父镜像），可以制作各种具体的应用镜像。

**特性：**
一次同时加载多个文件系统，但从外面看来，只能看到一个文件系统，联合加载会把各层文件系统叠加起来，这样最终的文件系统会包含所有底层的文件和目录。

![image-20220727174640983](D:\Learning\技术栈\Docker\Assests\docker3.png)



Docker镜像加载原理

- docker的镜像实际上由一层一层的文件系统组成，这种层级的文件系统UnionFS。
- bootfs(boot file system)主要包含bootloader和kernel, bootloader主要是引导加载kernel, Linux刚启动时会加载bootfs文件系统，在Docker镜像的最底层是boots。这一层与我们典型的Linux/Unix系统是一样的，包含boot加载器和内核。当boot加载完成之后整个内核就都在内存中了，此时内存的使用权已由bootfs转交给内核，此时系统也会卸载bootfs。
- roots (root fle system)，在bootfs之上。包含的就是典型Linux系统中的/dev,/proc, /bin, /etc等标准目录和文件。rootfs就是各种不同的操作系统发行版，比如Ubuntu ,Centos等等。

![在这里插入图片描述](D:\Learning\技术栈\Docker\Assests\docker4.jpg)

平时我们安装虚拟机的`CentOS`都是好几个G，为什么Docker才200M？

- 对于一个精简的OS，rootfs 可以很小，只需要包含最基本的命令，工具
- 和程序库就可以了，因为底层直接用Host的kernel自己只需要提供roots就可以了。由此可见对于不同的linux发行版, bootfs基本是一致的, rootfs会有差别,因此不同的发行版可以公用bootfs。
- 虚拟机是分钟级别，容器是秒级! 分层理解 `Docke`r的分层思想一层一层下载，逐层检测，存在即跳过，否则下载



### Docker分层结构

- 对于一个精简的OS，rootfs 可以很小，只需要包含最基本的命令，工具和程序库就可以了，因为底层直接用Host的kernel自己只需要提供roots就可以了。由此可见对于不同的linux发行版, bootfs基本是一致的, rootfs会有差别,因此不同的发行版可以公用bootfs。
- 虚拟机是分钟级别，容器是秒级! 分层理解 `Docke`r的分层思想一层一层下载，逐层检测，存在即跳过，否则下载















## 容器数据卷

Docker理解回顾

将应用和环境打包成一个镜像！

数据？如果数据都在容器中，那么我们容器删除，数据就会丢失！需求：数据可以持久化

MySQL，容器删了，删库跑路！需求：MySQL数据可以存储在本地！

容器之间可以有一个数据共享技术！Docker容器中产生的数据，同步到本地！

这就是卷技术，目录的挂载，将我们容器内的目录挂载到linux目录上面！

**总结： **容器的持久化和同步操作！容器间数据也是可以共享的！



#### 使用数据卷

直接通过使用命令挂载-v

```
#启动并进入容器
[root@Linux-Alex ~]# docker run -it centos /bin/bash 
#将主机目录和容器目录进行挂载
[root@Linux-Alex ~]# dockers run -it -v 主机目录:容器目录
#主机目录和容器目录进行挂载并进入容器
[root@Linux-Alex ~]# docker run -it -v /home/ceshi:/home centos /bin/bash

重新开个会话窗口执行（查看元数据）
[root@Linux-Alex home]# docker inspect eee3519ae31a
若元数据出现Mounts，则说明主机目录和容器目录挂载成功
"Mounts": [
            {
                "Type": "bind",
                "Source": "/home/ceshi", # 主机目录
                "Destination": "/home", # 容器目录
                "Mode": "",
                "RW": true,
                "Propagation": "rprivate"
            }
        ],

```



测试文件同步（在主机上改动，观察容器变化）

```
容器会话：
[root@Linux-Alex ~]# docker run -it -v /home/ceshi:/home centos /bin/bash	
[root@eee3519ae31a /]# cd /home
[root@eee3519ae31a home]# touch test.cs  #创建test.cs文件
```

```
主机会话：
[root@Linux-Alex home]# cd ceshi
[root@Linux-Alex ceshi]# ls
test.cs		# 发现当容器会话创建test.cs文件后，主机目录中也出现了相同文件
```

```
容器会话：
[root@eee3519ae31a home]# exit	#关闭当前容器
exit
[root@Linux-Alex ~]# docker ps	#查看当前运行的所有容器
CONTAINER ID   IMAGE                 COMMAND        CREATED        STATUS       PORTS                                                 NAMES
```

```
主机会话：
[root@Linux-Alex ceshi]# vim test.cs	# 在cs文件中添加内容  hello world
```

```
容器会话：
[root@Linux-Alex ~]# docker start eee3519ae31a		#启动容器
eee3519ae31a
[root@Linux-Alex ~]# docker attach eee3519ae31a		#进入当前正在运行的容器
[root@eee3519ae31a /]# cd home
[root@eee3519ae31a home]# cat test.java
hello		# 发现关闭容器后，依然可以获取主机目录中的添加的内容
```

**结论：**数据卷内容是双向同步的。



#### 实战：安装Mysql

如何实现Mysql的数据持久化问题？

```
# 获取镜像
[root@Linux-Alex ~]# docker pull mysql:5.7

# 运行容器，需要做数据挂载！
# 安装启动mysql，需要配置密码（注意）
# 官方测试， docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag
 
# 启动我们的
-d      # 后台运行
-p      # 端口隐射
-v      # 卷挂载
-e      # 环境配置
--name  # 容器的名字

[root@Linux-Alex ~]# docker run -d -p 3344:3306 -v /home/mysql/conf:/etc/mysql/conf.d -v /home/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 --name mysql01 mysql:5.7
05bc0db3c6ae9c1e4450b6ef9a7926cec676e6f6cd98db77169642418d083fb7

# 启动成功之后，我们在本地使用navicat链接测试一下
# navicat链接到服务器的3344 --- 3344 和 容器的3306映射，这个时候我们就可以连接上mysql喽！
 
# 在本地测试创建一个数据库，查看下我们的路径是否ok！
```

假设我们将容器删除

发现，我们挂载到本地的数据卷依旧没有丢失，这就实现了容器数据的持久化功能！



#### 匿名和具名挂载

```

```





## DockerFile

就是用来构建docker镜像的构建文件！命令脚本！

通过这个脚本可以生成镜像，镜像是一层一层的，脚本一个个的命令，每个命令都是一层

```
# 创建一个dockerfile文件， 名字可以随机
docker build -f dockerfile -t alex/centos:1.0

# 文件的内容 指定（大写） 参数
FROM centos
VOLUME ["volume01", "volume02"]
CMD echo "----end----"
CMD /bin/bash
 
# 这里的每一个命令都是镜像的一层！

# 查看镜像是否安装
[root@Linux-Alex docker-test-volume]# docker images
REPOSITORY            TAG       IMAGE ID       CREATED         SIZE
alex/centos           1.0       3776cdbc80ca   2 days ago      231MB

# 启动自己的容器
[root@Linux-Alex docker-test-volume]# docker run -it alex/centos:1.0 /bin/bash
[root@216b45b8c781 /]# ls -al
total 0
drwxr-xr-x.   1 root root  38 Aug  1 06:19 .
drwxr-xr-x.   1 root root  38 Aug  1 06:19 ..
-rwxr-xr-x.   1 root root   0 Aug  1 06:19 .dockerenv
lrwxrwxrwx.   1 root root   7 Nov  3  2020 bin -> usr/bin
drwxr-xr-x.   5 root root 360 Aug  1 06:19 dev
drwxr-xr-x.   1 root root  66 Aug  1 06:19 etc
drwxr-xr-x.   2 root root   6 Nov  3  2020 home
lrwxrwxrwx.   1 root root   7 Nov  3  2020 lib -> usr/lib
lrwxrwxrwx.   1 root root   9 Nov  3  2020 lib64 -> usr/lib64
drwx------.   2 root root   6 Sep 15  2021 lost+found
drwxr-xr-x.   2 root root   6 Nov  3  2020 media
drwxr-xr-x.   2 root root   6 Nov  3  2020 mnt
drwxr-xr-x.   2 root root   6 Nov  3  2020 opt
dr-xr-xr-x. 283 root root   0 Aug  1 06:19 proc
dr-xr-x---.   2 root root 162 Sep 15  2021 root
drwxr-xr-x.  11 root root 163 Sep 15  2021 run
lrwxrwxrwx.   1 root root   8 Nov  3  2020 sbin -> usr/sbin
drwxr-xr-x.   2 root root   6 Nov  3  2020 srv
dr-xr-xr-x.  13 root root   0 Aug  1 01:13 sys
drwxrwxrwt.   7 root root 171 Sep 15  2021 tmp
drwxr-xr-x.  12 root root 144 Sep 15  2021 usr
drwxr-xr-x.  20 root root 262 Sep 15  2021 var
drwxr-xr-x.   2 root root   6 Aug  1 06:19 volume01			
drwxr-xr-x.   2 root root   6 Aug  1 06:19 volume02				
# volume01，volume02就是我们生成镜像的时候自动挂宅数据卷目录
```

**查看挂载源**

```
# docker inspect 容器id
[root@Linux-Alex /]# docker inspect 6ae383b51b5d
---截取其中部分---
"Mounts": [
            {
                "Type": "volume",
                "Name": "027405d6edc784dac816c25fb1a69fb7f2119362250b97f7a1cf801499df0aa5",
                "Source": "/var/lib/docker/volumes/027405d6edc784dac816c25fb1a69fb7f2119362250b97f7a1cf801499df0aa5/_data",
                "Destination": "volume01",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            },
            {
                "Type": "volume",
                "Name": "2400b2340ac6e085564416b264903fe91c17326cdfb0f7ef14195a9e854ba02e",
                "Source": "/var/lib/docker/volumes/2400b2340ac6e085564416b264903fe91c17326cdfb0f7ef14195a9e854ba02e/_data",
                "Destination": "volume02",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            }
        ]
```

测试：新建文件，查看是否同步；

假设构建镜像时没有挂载卷，要手动镜像挂载 -v 卷名：容器内路径！

### 数据卷容器

多个mysql同步数据！















DockerFile

```
构建步骤
1. 编写一个dockerFile文件
2.docker build 构建成为一个镜像
3. docker run 运行镜像
4. docker push 发布镜像（DockerHub、阿里云镜像）
```

查看婴喜爱官方是怎么做的？









## Docker 网络



![image-20220801103945341](D:\Learning\技术栈\Docker\Assests\docker网络.png)

```
问题： 
#	docker是如何处理容器网络访问？


#	查看容器内部得网络地址 -- ip addr
[root@Linux-Alex /]# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:0c:29:0f:8c:41 brd ff:ff:ff:ff:ff:ff
    inet 192.168.253.132/24 brd 192.168.253.255 scope global noprefixroute dynamic ens33
       valid_lft 1687sec preferred_lft 1687sec
    inet 192.168.253.130/24 brd 192.168.253.255 scope global secondary noprefixroute ens33
       valid_lft forever preferred_lft forever
    inet6 fe80::6291:d202:f91f:11c0/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
3: virbr0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default qlen 1000
    link/ether 52:54:00:86:01:c7 brd ff:ff:ff:ff:ff:ff
    inet 192.168.122.1/24 brd 192.168.122.255 scope global virbr0
       valid_lft forever preferred_lft forever
4: virbr0-nic: <BROADCAST,MULTICAST> mtu 1500 qdisc pfifo_fast master virbr0 state DOWN group default qlen 1000
    link/ether 52:54:00:86:01:c7 brd ff:ff:ff:ff:ff:ff
5: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default 
    link/ether 02:42:87:95:21:e8 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever

#	Linux能否ping通容器？
[root@Linux-Alex /]# ping 172.17.0.1
PING 172.17.0.1 (172.17.0.1) 56(84) bytes of data.
64 bytes from 172.17.0.1: icmp_seq=1 ttl=64 time=0.036 ms
64 bytes from 172.17.0.1: icmp_seq=2 ttl=64 time=0.058 ms
64 bytes from 172.17.0.1: icmp_seq=3 ttl=64 time=0.048 ms
64 bytes from 172.17.0.1: icmp_seq=4 ttl=64 time=0.043 ms
```

**原理：**我们没启动docker容器，docker就会给docker容器分配一个IP，我们只要安装了docker，就会有一个网卡docker0桥接模式，使用的技术是veth-pair技术！























## Docker常用命令

### 查询CentOS版本命令

```
[root@Alex ~]# cat /etc/redhat-release
CentOS Linux release 7.9.2009 (Core)
```



### 容器命令

#### 查询容器id

```
# docker ps 命令
        # 列出当前正在运行的容器
-a      # 列出正在运行的容器包括历史容器
-n=?    # 显示最近创建的容器
-q      # 只显示当前容器的编号

[root@Linux-Alex /]# docker ps -a
CONTAINER ID   IMAGE             COMMAND                  CREATED         STATUS                       PORTS     NAMES
6ae383b51b5d   alex/centos:1.0   "/bin/bash"              8 minutes ago   Exited (127) 3 minutes ago             great_saha
7b39df3f0b73   3776cdbc80ca      "/bin/bash"              2 days ago      Exited (0) 2 days ago                  blissful_pare
df05c643cefc   3776cdbc80ca      "/bin/bash"              2 days ago      Exited (127) 2 days ago                pensive_gould
eee3519ae31a   centos            "/bin/bash"              4 days ago      Exited (130) 3 days ago                laughing_feistel
80ef470a6821   nginx             "/docker-entrypoint.…"   4 days ago      Exited (0) 4 days ago                  nginx01
e4f27dda251e   centos            "/bin/bash"              4 days ago      Exited (127) 4 days ago                peaceful_heisenberg
a8cb8db7eb7b   centos            "/bin/bash"              4 days ago      Exited (0) 4 days ago                  competent_mclean
def467d6ac56   centos            "/bin/bash"              4 days ago      Exited (127) 4 days ago                eloquent_diffie
e604ca46dd2d   centos            "/bin/bash"              4 days ago      Exited (0) 4 days ago                  wizardly_jones
4cdd4885c807   centos            "/bin/bash"              4 days ago      Exited (0) 4 days ago                  lucid_dubinsky
2853acb3c12c   centos            "/bin/bash"              4 days ago      Exited (0) 4 days ago                  condescending_meninsky
f6bbd3dadc84   centos            "/bin/bash"              4 days ago      Exited (127) 4 days ago                wizardly_swanson
1c3d80aa6e66   feb5d9fea6a5      "/hello"                 4 days ago      Exited (0) 4 days ago                  focused_mahavira
cce0063bfd94   feb5d9fea6a5      "/hello"                 4 days ago      Exited (0) 4 days ago                  keen_tesla
```

#### 退出容器

```
exit            # 直接退出容器并关闭
Ctrl + P + Q    # 容器不关闭退出
```

#### 删除容器

```
docker rm -f 容器id                       # 删除指定容器
docker rm -f $(docker ps -aq)       # 删除所有容器
docker ps -a -q|xargs docker rm -f  # 删除所有的容器
```

#### 启动和停止容器

```
docker start 容器id           # 启动容器
docker restart 容器id         # 重启容器
docker stop 容器id            # 停止当前正在运行的容器
docker kill 容器id            # 强制停止当前的容器
```



### 其它命令

#### 后台启动容器

```

```

#### 查看镜像的元数据

```
# 命令
docker inspect 容器id

[root@Linux-Alex /]# docker inspect 6ae383b51b5d
[
    {
        "Id": "6ae383b51b5d86ee0921961a2e0d63c7b8d835f58535d24b428811431b83a058",
        "Created": "2022-08-01T05:20:04.08667593Z",
        "Path": "/bin/bash",
        "Args": [],
        "State": {
            "Status": "exited",
            "Running": false,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 0,
            "ExitCode": 127,
            "Error": "",
            "StartedAt": "2022-08-01T05:20:04.780914812Z",
            "FinishedAt": "2022-08-01T05:25:23.785283486Z"
        },
        "Image": "sha256:3776cdbc80ca82e43cc6cbc891c3629bbca7b0c1289b68f57286def2d646351f",
        "ResolvConfPath": "/var/lib/docker/containers/6ae383b51b5d86ee0921961a2e0d63c7b8d835f58535d24b428811431b83a058/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/6ae383b51b5d86ee0921961a2e0d63c7b8d835f58535d24b428811431b83a058/hostname",
        "HostsPath": "/var/lib/docker/containers/6ae383b51b5d86ee0921961a2e0d63c7b8d835f58535d24b428811431b83a058/hosts",
        "LogPath": "/var/lib/docker/containers/6ae383b51b5d86ee0921961a2e0d63c7b8d835f58535d24b428811431b83a058/6ae383b51b5d86ee0921961a2e0d63c7b8d835f58535d24b428811431b83a058-json.log",
        "Name": "/great_saha",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
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
                "LowerDir": "/var/lib/docker/overlay2/915d156cbf73c4d8bab3f61729750243bb825df3aa2d0825ee54651ccd0dc33b-init/diff:/var/lib/docker/overlay2/3e1aaba2c8814006e28152ee0e671f295965c88c52722b2f7f6da9b4e6ecbf26/diff",
                "MergedDir": "/var/lib/docker/overlay2/915d156cbf73c4d8bab3f61729750243bb825df3aa2d0825ee54651ccd0dc33b/merged",
                "UpperDir": "/var/lib/docker/overlay2/915d156cbf73c4d8bab3f61729750243bb825df3aa2d0825ee54651ccd0dc33b/diff",
                "WorkDir": "/var/lib/docker/overlay2/915d156cbf73c4d8bab3f61729750243bb825df3aa2d0825ee54651ccd0dc33b/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [
            {
                "Type": "volume",
                "Name": "027405d6edc784dac816c25fb1a69fb7f2119362250b97f7a1cf801499df0aa5",
                "Source": "/var/lib/docker/volumes/027405d6edc784dac816c25fb1a69fb7f2119362250b97f7a1cf801499df0aa5/_data",
                "Destination": "volume01",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            },
            {
                "Type": "volume",
                "Name": "2400b2340ac6e085564416b264903fe91c17326cdfb0f7ef14195a9e854ba02e",
                "Source": "/var/lib/docker/volumes/2400b2340ac6e085564416b264903fe91c17326cdfb0f7ef14195a9e854ba02e/_data",
                "Destination": "volume02",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            }
        ],
        "Config": {
            "Hostname": "6ae383b51b5d",
            "Domainname": "",
            "User": "",
            "AttachStdin": true,
            "AttachStdout": true,
            "AttachStderr": true,
            "Tty": true,
            "OpenStdin": true,
            "StdinOnce": true,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/bash"
            ],
            "Image": "alex/centos:1.0",
            "Volumes": {
                "volume01": {},
                "volume02": {}
            },
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
            "SandboxID": "074023a6ab6498d0a07df29ac0a86dccff1520707871af110e9afd5397a0ac62",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/074023a6ab64",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "",
            "Gateway": "",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "",
            "IPPrefixLen": 0,
            "IPv6Gateway": "",
            "MacAddress": "",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "6fda9f58a13308fc86e486c1912e7a8143a90441a1bdb3292128b2605fc6adfc",
                    "EndpointID": "",
                    "Gateway": "",
                    "IPAddress": "",
                    "IPPrefixLen": 0,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "",
                    "DriverOpts": null
                }
            }
        }
    }
]
```





## 参考文献

B栈狂神说docker：https://www.bilibili.com/video/BV1og4y1q7M4

Docker官方文档：https://docs.docker.com/

配置阿里云镜像加速：[(118条消息) docker配置阿里云镜像加速（官方指南）_王钧石的技术博客的博客-CSDN博客_阿里云docker镜像加速](https://blog.csdn.net/weixin_43830765/article/details/123519840)

Docker命令帮助文档：https://docs.docker.com/engine/reference/commandline/docker/