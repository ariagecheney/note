Docker 运行在 CentOS-6.5 或更高的版本的 CentOS 上，需要内核版本是 2.6.32-431 或者更高版本 ，因为这是允许它运行的指定内核补丁版本。
# [Docker 的版本演进](https://docs.lvrui.io/2017/03/06/Docker-%E7%9A%84%E7%89%88%E6%9C%AC%E6%BC%94%E8%BF%9B/)
# [debain安装](https://yeasy.gitbooks.io/docker_practice/install/debian.html)

```sh
sudo apt-get update

sudo apt-get install \
     apt-transport-https \
     ca-certificates \
     curl \
     gnupg2 \
     lsb-release \
     software-properties-common

```
* [deepin 安装docker](https://wiki.deepin.org/index.php?title=Docker)
* [免sudo使用docker命令](https://www.jianshu.com/p/95e397570896)

# [CentOS-6.8安装Docker1.7.1](https://www.jianshu.com/p/4e74f11ee309)
安装完成后，运行下面的命令，验证是否安装成功。


$ docker version  
$ docker info  
* man  
docker   
docker COMMAND --h  
 
* [国内镜像](http://icyleaf.com/2016/12/docker-with-centos/)
```sh
# /etc/sysconfig/docker
 #
 # Other arguments to pass to the docker daemon process
 # These will be parsed by the sysv initscript and appended
 # to the arguments list passed to docker -d
 
 #other_args="--registry-mirror=http://hub-mirror.c.163.com"
 other_args="--registry-mirror=https://docker.mirrors.ustc.edu.cn"
 
 DOCKER_CERT_PATH=/etc/docker
 
 # Resolves: rhbz#1176302 (docker issue #407)
 DOCKER_NOWARN_KERNEL_VERSION=1
 
 # Location used for temporary files, such as those created by
 # # docker load and build operations. Default is /var/lib/docker/tmp
 # # Can be overriden by setting the following environment variable.
 # # DOCKER_TMPDIR=/var/tmp

```

Docker 是服务器----客户端架构。命令行运行docker命令的时候，需要本机有 Docker 服务。如果这项服务没有启动，可以用下面的命令启动（官方文档）。


*  service 命令的用法
$ sudo service docker start

*  systemctl 命令的用法
$ sudo systemctl start docker

或者  
`/usr/bin/docker -d --registry-mirror=https://docker.mirrors.ustc.edu.cn`

## [容器和镜像](http://dockone.io/article/783)

* 容器存储层  

前面讲过镜像使用的是分层存储，容器也是如此。每一个容器运行时，是以镜像为基础层，在其上创建一个当前容器的存储层，我们可以称这个为容器运行时读写而准备的存储层为容器存储层。

容器存储层的生存周期和容器一样，容器消亡时，容器存储层也随之消亡。因此，任何保存于容器存储层的信息都会随容器删除而丢失。

按照 Docker 最佳实践的要求，容器不应该向其存储层内写入任何数据，容器存储层要保持无状态化。所有的文件写入操作，都应该使用 数据卷（Volume）、或者绑定宿主目录，在这些位置的读写会跳过容器存储层，直接对宿主（或网络存储）发生读写，其性能和稳定性更高。

数据卷的生存周期独立于容器，容器消亡，数据卷不会消亡。因此，使用数据卷后，容器删除或者重新运行之后，数据却不会丢失。

docker search centos

docker pull centos


docker images

docker ps -a

docker build -t="xianhu/centos:gitdir" .

其中-t用来指定新镜像的用户信息、tag等。最后的点表示在当前目录寻找Dockerfile。

可以使用rm命令，注意：删除镜像前必须先删除以此镜像为基础的容器。

[root@xxx ~]# docker rm container_name/container_id
[root@xxx ~]# docker rmi image_name/image_id

镜像其他操作指令：

[root@xxx ~]# docker save -o centos.tar xianhu/centos:git    # 保存镜像, -o也可以是--output
[root@xxx ~]# docker load -i centos.tar    # 加载镜像, -i也可以是--input

Docker中关于容器的基本操作
在前边镜像的章节中，我们已经看到了如何基于镜像启动一个容器，即docker run操作。
[root@xxx ~]# docker run -it centos:latest /bin/bash
这里-it是两个参数：-i和-t。前者表示打开并保持stdout，后者表示分配一个终端（pseudo-tty）。此时如果使用exit退出，则容器的状态处于Exit，而不是后台运行。如果想让容器一直运行，而不是停止，可以使用快捷键 ctrl+p ctrl+q 退出，此时容器的状态为Up。

除了这两个参数之外，run命令还有很多其他参数。其中比较有用的是-d后台运行：


[root@xxx ~]# docker run centos:latest /bin/bash -c "while true; do echo hello; sleep 1; done"
[root@xxx ~]# docker run -d centos:latest /bin/bash -c "while true; do echo hello; sleep 1; done"
这里第二条命令使用了-d参数，使这个容器处于后台运行的状态，不会对当前终端产生任何输出，所有的stdout都输出到log，可以使用docker logs container_name/container_id查看。

启动、停止、重启容器命令：

[root@xxx ~]# docker start container_name/container_id
[root@xxx ~]# docker stop container_name/container_id
[root@xxx ~]# docker restart container_name/container_id
后台启动一个容器后，如果想进入到这个容器，可以使用attach命令：

[root@xxx ~]# docker attach container_name/container_id
删除容器的命令前边已经提到过了：

[root@xxx ~]# docker rm container_name/container_id
# 命令
```sh
容器生命周期管理
run
start/stop/restart
kill
rm
pause/unpause
create
exec
容器操作
ps
inspect
top
attach
events
logs
wait
export
port
容器rootfs命令
commit
cp
diff
镜像仓库
login
pull
push
search
本地镜像管理
images
rmi
tag
build
history
save
import
info|version
info
version
```