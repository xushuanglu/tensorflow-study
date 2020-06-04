Tensorflow 学习

docker之配置TensorFlow的运行环境

Docker是一种 操作系统层面的虚拟化技术，类似于传统的虚拟机。传统虚拟机技术是虚拟出一套硬件后，在其上运行一个完整操作系统，在该系统上再运行所需应用进程；而容器内的应用进程直接运行于宿主的内核，容器内没有自己的内核，而且也没有进行硬件虚拟。因此容器要比传统虚拟机更为轻便。传统虚拟机使用前需要指定内存、硬盘灯大小，使用过程中即使程序没有占用那么多资源也不会释放出来，而Docker则是使用多少则占用多少。

Docker有三个主要的概念：镜像（Image）、容器（Container）、仓库（Repository）。我们都知道，操作系统分为内核和用户空间。对于 Linux 而言，内核启动后，会挂载 root 文件系统为其提供用户空间支持。而 Docker 镜像，就相当于是一个 root 文件系统。比如官方镜像 ubuntu:16.04 就包含了完整的一套 Ubuntu 16.04 最小系统的 root 文件系统。

Docker 镜像是一个特殊的文件系统，除了提供容器运行时所需的程序、库、资源、配置等文件外，还包含了一些为运行时准备的一些配置参数（如匿名卷、环境变量、用户等）。镜像不包含任何动态数据，其内容在构建之后也不会被改变。Docker中的镜像是分层存储，往往是不同的docker依赖同一个的文件。

镜像和容器的关系，就像是面向对象程序设计中的 类 和 实例 一样，镜像是静态的定义，容器是镜像运行时的实体。容器可以被创建、启动、停止、删除、暂停等。

容器的实质是进程，但与直接在宿主执行的进程不同，容器进程运行于属于自己的独立的 命名空间。因此容器可以拥有自己的 root 文件系统、自己的网络配置、自己的进程空间，甚至自己的用户 ID 空间。容器内的进程是运行在一个隔离的环境里，使用起来，就好像是在一个独立于宿主的系统下操作一样。这种特性使得容器封装的应用比直接在宿主运行更加安全。也因为这种隔离的特性，很多人初学 Docker 时常常会混淆容器和虚拟机。

前面讲过镜像使用的是分层存储，容器也是如此。每一个容器运行时，是以镜像为基础层，在其上创建一个当前容器的存储层，我们可以称这个为容器运行时读写而准备的存储层为容器存储层。

容器存储层的生存周期和容器一样，容器消亡时，容器存储层也随之消亡。因此，任何保存于容器存储层的信息都会随容器删除而丢失。

仓库是存放镜像的地方，一个仓库可以有多个,这样便于集中管理。

在熟悉上述概念的情况下，接下来将记述如何在docker中配置可以运行TensorFlow的环境。

(前提已经安装好docker，想要运行TensorFlow代码)

1、    运行一个你熟悉的镜像(如Ubuntu系统的镜像)

docker run –it images_id /bin/bash

2、    查询正在运行的容器

docker ps

3、    进入你之前运行的容器中

docker exec –it container_id /bin/bash

(这样可以使用exit退出你的容器而不关闭容器)

4、    安装python

apt update           //更新ubuntu系统

apt install python3  //安装python3

5、    安装pip3

apt install python3-pip

6、    安装TensorFlow

pip install tensorflow

如此，服务器中的TensorFlow的运行环境就配好了。

提示：如果你的Docker中,无法使用中文，

首先，在docker 查看你的语言环境，输入:  locale

发现都是“POSIX”的话表示，此时的语言环境不能输入中文。接着查看支持的语言环境，输入 locale -a

如发现支持C.UTF-8，那么在进入容器时，使用如下命令：

docker docker exec -it  container_id env LANG=C.UTF-8 bash

这样就可以了。(PS:如果容器内不支持中文，哪怕你直接在python中处理中文的语料也会报错！)

不建议直接安装带有TensorFlow的镜像，主要是这些镜像里面的设置都配置好了，不太容易更改，自己安装的话，可以更好地控制版本。
