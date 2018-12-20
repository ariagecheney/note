## [参考：Docker — 从入门到实践](https://docker_practice.gitee.io/)
# [Docker 的版本演进](https://docs.lvrui.io/2017/03/06/Docker-%E7%9A%84%E7%89%88%E6%9C%AC%E6%BC%94%E8%BF%9B/)
# [debain安装](https://yeasy.gitbooks.io/docker_practice/install/debian.html)

* [deepin 安装docker](https://wiki.deepin.org/index.php?title=Docker)
* [免sudo使用docker命令](https://www.jianshu.com/p/95e397570896)

## debain(deepin)
```sh
sudo aptitude update
sudo aptitude install apt-transport-https ca-certificates curl gnupg2 lsb-release software-properties-common
curl -fsSL https://mirrors.ustc.edu.cn/docker-ce/linux/debian/gpg | sudo apt-key add - 
sudo apt-key finger 0ebfcd88
sudo add-apt-repository \                                                             
   "deb [arch=amd64] https://mirrors.ustc.edu.cn/docker-ce/linux/debian \
   $(lsb_release -cs) \
   stable"
# deepin 把 $(lsb_release -cs) jessie
sudo aptitude update
sudo aptitude install docker-ce
# 开机启动文件  /etc/systemd/system/multi-user.target.wants/docker.service → /lib/systemd/system/docker.service
sudo systemctl enable docker
sudo systemctl start docker

## 国内镜像加速器
sudo vim /etc/docker/daemon.json 文件
{
  "registry-mirrors": [
    "https://registry.docker-cn.com"
  ]
}
重载所有修改过的配置文件
$ sudo systemctl daemon-reload
sudo systemctl restart docker
docker info
## 建立 docker 用户组
sudo groupadd docker
sudo usermod -aG docker $USER

```
## 镜像，容器，仓库
```sh
sudo docker pull user/ubuntu:latest
docker run -it --rm \
    ubuntu:16.04 \
    bash
    一个镜像可以对应多个标签
sudo docker image ls
docker system df
docker image ls -f dangling=true
docker image prune

中间层镜像
docker image ls -a
列出部分镜像
docker image ls [OPTIONS] [REPOSITORY[:TAG]]

docker image ls -f since=mongo:3.2  #before
以特定格式显示
docker image ls --format "{{.ID}}: {{.Repository}}"
docker image ls --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}"

docker image rm [选项] <镜像1> [<镜像2> ...]

镜像短 ID、镜像长 ID、镜像名 或者 镜像摘要。

docker image ls --digests
docker image rm node@sha256:b4f0e0bdeb578043c1ea6862f0d40cc4afe32a4a582f3be235a3b164422be228

一类是 Untagged，另一类是 Deleted
实际上是在要求删除某个标签的镜像
当该镜像所有的标签都被取消了，该镜像很可能会失去了存在的意义，因此会触发删除行为。镜像是多层存储结构，因此在删除的时候也是从上层向基础层方向依次进行判断删除。镜像的多层结构让镜像复用变动非常容易，因此很有可能某个其它镜像正依赖于当前镜像的某一层。这种情况，依旧不会触发删除该层的行为。直到没有任何层依赖当前层时，才会真实的删除当前层。
除了镜像依赖以外，还需要注意的是容器对镜像的依赖。如果有用这个镜像启动的容器存在（即使容器没有运行），那么同样不可以删除这个镜像。之前讲过，容器是以镜像为基础，再加一层容器存储层，组成这样的多层存储结构去运行的。因此该镜像如果被这个容器所依赖的，那么删除必然会导致故障。如果这些容器是不需要的，应该先将它们删除，然后再来删除镜像。
docker image rm $(docker image ls -q redis)
docker image rm $(docker image ls -q -f before=mongo:3.2)
## 定制
docker commit \
    --author "Tao Wang <twang2218@gmail.com>" \
    --message "修改了默认网页" \
    webserver \
    nginx:v2

docker history nginx:v2

docker run --name web2 -d -p 81:80 nginx:v2

RUN 指令是用来执行命令行命令的。由于命令行的强大能力，RUN 指令在定制镜像时是最常用的指令之一。其格式有两种：

shell 格式：RUN <命令>，就像直接在命令行中输入的命令一样。刚才写的 Dockerfile 中的 RUN 指令就是这种格式。
RUN echo '<h1>Hello, Docker!</h1>' > /usr/share/nginx/html/index.html
exec 格式：RUN ["可执行文件", "参数1", "参数2"]，这更像是函数调用中的格式。

docker build [选项] <上下文路径/URL/->

Dockerfile 中的 ARG 指令是定义参数名称，以及定义其默认值。该默认值可以在构建命令 docker build 中用 --build-arg <参数名>=<值> 来覆盖。

docker build -t nginx:v3 .

一般来说，应该会将 Dockerfile 置于一个空目录下，或者项目根目录下。如果该目录下没有所需文件，那么应该把所需文件复制一份过来。如果目录下有些东西确实不希望构建时传给 Docker 引擎，那么可以用 .gitignore 一样的语法写一个 .dockerignore，该文件是用于剔除不需要作为上下文传递给 Docker 引擎的。
COPY 
<源路径> 可以是多个，甚至可以是通配符，其通配符规则要满足 Go 的 filepath.Match 规则，如：
pattern:
	{ term }
term:
	'*'         matches any sequence of non-Separator characters
	'?'         matches any single non-Separator character
	'[' [ '^' ] { character-range } ']'
	            character class (must be non-empty)
	c           matches character c (c != '*', '?', '\\', '[')
	'\\' c      matches character c

character-range:
	c           matches character c (c != '\\', '-', ']')
	'\\' c      matches character c
	lo '-' hi   matches character c for lo <= c <= hi

<目标路径> 可以是容器内的绝对路径，也可以是相对于工作目录的相对路径（工作目录可以用 WORKDIR 指令来指定）。目标路径不需要事先创建，如果目录不存在会在复制文件前先行创建缺失目录。
所有的文件复制均使用 COPY 指令，仅在需要自动解压缩的场合使用 ADD。

如 <源路径> 可以是一个 URL，这种情况下，Docker 引擎会试图去下载这个链接的文件放到 <目标路径> 去。下载后的文件权限自动设置为 600，如果这并不是想要的权限，那么还需要增加额外的一层 RUN 进行权限调整，另外，如果下载的是个压缩包，需要解压缩，也一样还需要额外的一层 RUN 指令进行解压缩。所以不如直接使用 RUN 指令，然后使用 wget 或者 curl 工具下载，处理权限、解压缩、然后清理无用文件更合理。因此，这个功能其实并不实用，而且不推荐使用。

容器内没有后台服务的概念

如果 <源路径> 为一个 tar 压缩文件的话，压缩格式为 gzip, bzip2 以及 xz 的情况下，ADD 指令将会自动解压缩这个压缩文件到 <目标路径> 去。

CMD 指定容器启动程序及参数 ,CMD指令就是用于指定默认的容器主进程的启动命令的
对于容器而言，其启动程序就是容器应用进程，容器就是为了主进程而存在的，主进程退出，容器就失去了存在的意义，从而退出，其它辅助进程不是它需要关心的东西。

而使用 service nginx start 命令，则是希望 upstart 来以后台守护进程形式启动 nginx 服务。而刚才说了 CMD service nginx start 会被理解为 CMD [ "sh", "-c", "service nginx start"]，因此主进程实际上是 sh。那么当 service nginx start 命令结束后，sh 也就结束了，sh 作为主进程退出了，自然就会令容器退出。

CMD ["nginx", "-g", "daemon off;"]

docker run image cmd arg
run 命令，跟在镜像名后面的是 command，运行时会替换 CMD 的默认值。 
当存在 ENTRYPOINT 后，CMD 的内容将会作为参数传给 ENTRYPOINT

这些准备工作是和容器 CMD 无关的，无论 CMD 为什么，都需要事先进行一个预处理的工作。这种情况下，可以写一个脚本，然后放入 ENTRYPOINT 中去执行，而这个脚本会将接到的参数（也就是 <CMD>）作为命令，在脚本最后执行。

构建参数和 ENV 的效果一样，都是设置环境变量。所不同的是，ARG 所设置的构建环境的环境变量，在将来容器运行时是不会存在这些环境变量的。但是不要因此就使用 ARG 保存密码之类的信息，因为 docker history 还是可以看到所有值的。

Dockerfile 中的 ARG 指令是定义参数名称，以及定义其默认值。该默认值可以在构建命令 docker build 中用 --build-arg <参数名>=<值> 来覆盖。

VOLUME /data
这里的 /data 目录就会在运行时自动挂载为匿名卷，任何向 /data 中写入的信息都不会记录进容器存储层，从而保证了容器存储层的无状态化。当然，运行时可以覆盖这个挂载设置。比如：

docker run -d -v mydata:/data xxxx

EXPOSE 指令是声明运行时容器提供服务端口，这只是一个声明，在运行时并不会因为这个声明应用就会开启这个端口的服务。在 Dockerfile 中写入这样的声明有两个好处，一个是帮助镜像使用者理解这个镜像服务的守护端口，以方便配置映射；另一个用处则是在运行时使用随机端口映射时，也就是 docker run -P 时，会自动随机映射 EXPOSE 的端口。

之前说过每一个 RUN 都是启动一个容器、执行命令、然后提交存储层文件变更。第一层 RUN cd /app 的执行仅仅是当前进程的工作目录变更，一个内存上的变化而已，其结果不会造成任何文件变更。而到第二层的时候，启动的是一个全新的容器，跟第一层的容器更完全没关系，自然不可能继承前一层构建过程中的内存变化。

因此如果需要改变以后各层的工作目录的位置，那么应该使用 WORKDIR 指令。

格式：USER <用户名>[:<用户组>]

USER 指令和 WORKDIR 相似，都是改变环境状态并影响以后的层。WORKDIR 是改变工作目录，USER 则是改变之后层的执行 RUN, CMD 以及 ENTRYPOINT 这类命令的身份。

当然，和 WORKDIR 一样，USER 只是帮助你切换到指定用户而已，这个用户必须是事先建立好的，否则无法切换。

HEALTHCHECK [选项] CMD <命令>：设置检查容器健康状况的命令
HEALTHCHECK NONE：如果基础镜像有健康检查指令，使用这行可以屏蔽掉其健康检查指令

HEALTHCHECK 支持下列选项：

--interval=<间隔>：两次健康检查的间隔，默认为 30 秒；
--timeout=<时长>：健康检查命令运行超时时间，如果超过这个时间，本次健康检查就被视为失败，默认 30 秒；
--retries=<次数>：当连续失败指定次数后，则将容器状态视为 unhealthy，默认 3 次。
和 CMD, ENTRYPOINT 一样，HEALTHCHECK 只可以出现一次，如果写了多个，只有最后一个生效。

格式：ONBUILD <其它指令>。

ONBUILD 是一个特殊的指令，它后面跟的是其它指令，比如 RUN, COPY 等，而这些指令，在当前镜像构建时并不会被执行。只有当以当前镜像为基础镜像，去构建下一级镜像的时候才会被执行。

只构建某一阶段的镜像
我们可以使用 as 来为某一阶段命名，例如

FROM golang:1.9-alpine as builder
例如当我们只想构建 builder 阶段的镜像时，我们可以在使用 docker build 命令时加上 --target 参数即可

$ docker build --target builder -t username/imagename:tag .

构建时从其他镜像复制文件
上面例子中我们使用 COPY --from=0 /go/src/github.com/go/helloworld/app . 从上一阶段的镜像中复制文件，我们也可以复制任意镜像中的文件。

$ COPY --from=nginx:latest /etc/nginx/nginx.conf /nginx.conf

docker save [OPTIONS] IMAGE [IMAGE...]

docker save alpine | gzip > alpine-latest.tar.gz

docker load -i alpine-latest.tar.gz
# 容器

简单的说，容器是独立运行的一个或一组应用，以及它们的运行态环境

docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
当利用 docker run 来创建容器时，Docker 在后台运行的标准操作包括：

检查本地是否存在指定的镜像，不存在就从公有仓库下载
利用镜像创建并启动一个容器
分配一个文件系统，并在只读的镜像层外面挂载一层可读写层
从宿主主机配置的网桥接口中桥接一个虚拟接口到容器中去
从地址池配置一个 ip 地址给容器
执行用户指定的应用程序
执行完毕后容器被终止

docker container COMMAND

容器的核心为所执行的应用程序，所需要的资源都是应用程序运行所必需的。除此之外，并没有其它的资源。可以在伪终端中利用 ps 或 top 来查看进程信息。

容器是否会长久运行，是和 docker run 指定的命令有关，和 -d 参数无关

-d 参数启动后会返回一个唯一的 id，也可以通过 docker container ls 命令来查看容器信息。
docker container logs

终止状态的容器可以用 docker container ls -a 命令看到

在使用 -d 参数时，容器启动后会进入后台。如果想让容器一直运行，而不是停止，可以使用快捷键 ctrl+p ctrl+q 退出，此时容器的状态为Up。

* 某些时候需要进入容器进行操作，包括使用 docker attach 命令或 docker exec 命令
attach 与 exec 主要区别如下:

attach 直接进入容器 启动命令 的终端，不会启动新的进程, Ctrl+p 然后 Ctrl+q 组合键退出 attach 终端。
exec 则是在容器中打开新的终端，并且可以启动新的进程。
docker exec [OPTIONS] CONTAINER COMMAND [ARG...]

docker exec -it 69d1 bash

如果从这个 stdin 中 exit，不会导致容器的停止

如果想直接在终端中查看启动命令的输出，用 attach；其他情况使用 exec。

当然，如果只是为了查看启动命令的输出，可以使用 docker logs 命令

* 可以使用rm命令，注意：删除镜像前必须先删除以此镜像为基础的容器。

docker rm container_name/container_id
docker rmi image_name/image_id

docker export 7691a814370e > ubuntu.tar
docker import [OPTIONS] file|URL|- [REPOSITORY[:TAG]]

用户既可以使用 docker load 来导入镜像存储文件到本地镜像库，也可以使用 docker import 来导入一个容器快照到本地镜像库。这两者的区别在于容器快照文件将丢弃所有的历史记录和元数据信息（即仅保存容器当时的快照状态），而镜像存储文件将保存完整记录，体积也要大。此外，从容器快照文件导入时可以重新指定标签等元数据信息。

可以使用 docker container rm 来删除一个处于终止状态的容器

如果要删除一个运行中的容器，可以添加 -f 参数。Docker 会发送 SIGKILL 信号给容器。
用 docker container ls -a 命令可以查看所有已经创建的包括终止状态的容器，如果数量太多要一个个删除可能会很麻烦，用下面的命令可以清理掉所有处于终止状态的容器。

$ docker container prune

# 数据管理
docker volume create my-vol
docker volume ls
docker volume inspect my-vol
docker volume rm my-vol

可以在删除容器的时候使用 docker rm -v 这个命令。

无主的数据卷可能会占据很多空间，要清理请使用以下命令

docker volume prune

docker run -d -P \
    --name web \
    # -v my-vol:/wepapp \
    --mount source=my-vol,target=/webapp \
    training/webapp \
    python app.py

docker run -d -P \
    --name web \
    # -v /src/webapp:/opt/webapp \
    --mount type=bind,source=/src/webapp,target=/opt/webapp \
    training/webapp \
    python app.py

docker run -d -P \
    --name web \
    # -v /src/webapp:/opt/webapp:ro \
    --mount type=bind,source=/src/webapp,target=/opt/webapp,readonly \
    training/webapp \
    python app.py

VOLUME 定义匿名卷
格式为：

VOLUME ["<路径1>", "<路径2>"...]
VOLUME <路径>

docker run -d -v mydata:/data xxxx

# 网络
格式为 EXPOSE <端口1> [<端口2>...]。

EXPOSE 指令是声明运行时容器提供服务端口，这只是一个声明，在运行时并不会因为这个声明应用就会开启这个端口的服务。在 Dockerfile 中写入这样的声明有两个好处，一个是帮助镜像使用者理解这个镜像服务的守护端口，以方便配置映射；另一个用处则是在运行时使用随机端口映射时，也就是 docker run -P 时，会自动随机映射 EXPOSE 的端口。

此外，在早期 Docker 版本中还有一个特殊的用处。以前所有容器都运行于默认桥接网络中，因此所有容器互相之间都可以直接访问，这样存在一定的安全性问题。于是有了一个 Docker 引擎参数 --icc=false，当指定该参数后，容器间将默认无法互访，除非互相间使用了 --links 参数的容器才可以互通，并且只有镜像中 EXPOSE 所声明的端口才可以被访问。这个 --icc=false 的用法，在引入了 docker network 后已经基本不用了，通过自定义网络可以很轻松的实现容器间的互联与隔离。

要将 EXPOSE 和在运行时使用 -p <宿主端口>:<容器端口> 区分开来。-p，是映射宿主端口和容器端口，换句话说，就是将容器的对应端口服务公开给外界访问，而 EXPOSE 仅仅是声明容器打算使用什么端口而已，并不会自动在宿主进行端口映射。

-p 则可以指定要映射的端口，并且，在一个指定端口上只可以绑定一个容器。支持的格式有 ip:hostPort:containerPort/udp | ip::containerPort | hostPort:containerPort。

docker port CONTAINER [PRIVATE_PORT[/PROTO]]

docker port cid 5000

docker run -d \
    -p 5000:5000 \
    -p 3000:80 \
    training/webapp \
    python app.py

docker network create -d bridge my-net
-d 参数指定 Docker 网络类型，有 bridge overlay。其中 overlay 网络类型用于 Swarm mode，在本小节中你可以忽略它。

docker run -it --rm --name busybox1 --network my-net busybox sh

配置全部容器的 DNS ，也可以在 /etc/docker/daemon.json 文件中增加以下内容来设置。

{
  "dns" : [
    "114.114.114.114",
    "8.8.8.8"
  ]
}

-h HOSTNAME 或者 --hostname=HOSTNAME 设定容器的主机名，它会被写到容器内的 /etc/hostname 和 /etc/hosts。但它在容器外部看不到，既不会在 docker container ls 中显示，也不会在其他的容器的 /etc/hosts 看到。

--dns=IP_ADDRESS 添加 DNS 服务器到容器的 /etc/resolv.conf 中，让容器用这个服务器来解析所有不在 /etc/hosts 中的主机名。

--dns-search=DOMAIN 设定容器的搜索域，当设定搜索域为 .example.com 时，在搜索一个名为 host 的主机时，DNS 不仅搜索 host，还会搜索 host.example.com。

注意：如果在容器启动时没有指定最后两个参数，Docker 会默认用主机上的 /etc/resolv.conf 来配置容器。
```

Docker 运行在 CentOS-6.5 或更高的版本的 CentOS 上，需要内核版本是 2.6.32-431 或者更高版本 ，因为这是允许它运行的指定内核补丁版本。

# [centos7 安装](https://docs.docker.com/install/linux/docker-ce/centos/#install-from-a-package) 
* https://download.docker.com/linux/centos/7/x86_64/stable/Packages/
```sh
yum install docker-ce-selinux-17.03.3.ce-1.el7.noarch.rpm 
yum install docker-ce-17.03.3.ce-1.el7.x86_64.rpm
```

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



# 命令
```sh
# 批量清理已停止的容器
docker rm -f $(docker ps -qa)

# 删除没有标签的镜像
docker rmi $(docker images | grep "^<none>" | awk "{print $3}")


# 批量删除孤单的 volumes
docker volume ls -qf dangling=true
docker volume rm $(docker volume ls -qf dangling=true)

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

# Docker Machine
## 前提
* 需要配置root到服务端的无密码ssh登录。可以通过ssh-keygen、ssh-copy-id等命令实现
* 如果配置无密码登录的用户不是root用户的话，还需要在服务端上实现普通用户的无密码sudo权限。参考下面
```sh

sudo adduser nick  
sudo usermod -a -G sudo nick   
sudo visudo  

把下面一行内容添加到文档的最后并保存文件：

nick   ALL=(ALL:ALL) NOPASSWD: ALL

ssh-copy-id eversec@192.168.200.67


base=https://github.com/docker/machine/releases/download/v0.16.0 && curl -L $base/docker-machine-$(uname -s)-$(uname -m) >/tmp/docker-machine && sudo install /tmp/docker-machine /usr/local/bin/docker-machine
```
* [generic](https://docs.docker.com/machine/drivers/generic/)
```sh
docker-machine create \
  --driver generic \
  --generic-ip-address=203.0.113.81 \
  --generic-ssh-key ~/.ssh/id_rsa \
  hostname
```

* [ssh登录很慢解决方法](https://www.linuxprobe.com/ssh-login-slowly.html)
## 如何更改 Docker 的默认存储位置？

mv /var/lib/docker /storage/
ln -s /storage/docker/ /var/lib/docker