# 快捷键
* Ctrl+d	键盘输入结束或退出终端
* Ctrl+s	暂定当前程序，暂停后按下任意键恢复运行
* Ctrl+z	将当前程序放到后台运行，恢复到前台为命令fg
* Ctrl+a	将光标移至输入行头，相当于Home键
* Ctrl+e	将光标移至输入行末，相当于End键
* Ctrl+k	剪切从光标所在位置到行末 Ctrl+u
* ctrl + y 粘贴
* ctrl+l 清屏
* ALT + B - 跳回上一个空格
* Alt+Backspace	向前删除一个单词
* CTRL + W - 剪切光标前一个单词
* Shift+PgUp	将终端显示向上滚动
* Shift+PgDn	将终端显示向下滚动
* crtl + alt + 方向键 ：工作区切换
* shift + crtl + alt + 方向键 ：把当前窗口移到另一个工作区
* super + d ：显示桌面
* shift + f10 ： 鼠标右键
* ctrl + alt + backspace ： 重启会话
* super + l ： 锁定屏幕
* alt + f7 ： 激活移动窗口功能，方向键移动，enter键确定
* alt + enter ： 显示文件属性

Shell 常用通配符：

字符	含义
*	匹配 0 或多个字符
?	匹配任意一个字符
[list]	匹配 list 中的任意单一字符
[!list]	匹配 除list 中的任意单一字符以外的字符
[c1-c2]	匹配 c1-c2 中的任意单一字符 如：[0-9] [a-z]
{string1,string2,...}	匹配 sring1 或 string2 (或更多)其一字符串
{c2..c2}	匹配 c1-c2 中全部字符 如{1..10}


在Research UNIX、BSD、OS X 和 Linux 中，手册通常被分为8个区段，安排如下：

区段	说明
1	一般命令
2	系统调用
3	库函数，涵盖了C标准函数库
4	特殊文件（通常是/dev中的设备）和驱动程序
5	文件格式和约定
6	游戏和屏保
7	杂项
8	系统管理命令和守护进程
要查看相应区段的内容，就在 man 后面加上相应区段的数字即可，如：

$ man 3 printf


通常 man 手册中的内容很多，你可能不太容易找到你想要的结果，不过幸运的是你可以在 man 中使用搜索，/<你要搜索的关键字>，查找到后你可以使用n键切换到下一个关键字所在处，shift+n为上一个关键字所在处。使用Space(空格键)翻页，Enter(回车键)向下滚动一行，或者使用j,k（vim编辑器的移动键）进行向前向后滚动一行。按下h键为显示使用帮助(因为man使用less作为阅读器，实为less工具的帮助)，按下q退出。

想要获得更详细的帮助，你还可以使用info命令，不过通常使用man就足够了。如果你知道某个命令的作用，只是想快速查看一些它的某个具体参数的作用，那么你可以使用--help参数，大部分命令都会带有这个参数，如：

whoami
打印当前用户
who am i
当前终端登录用户
pwd
打印当前目录
exit
退出当前用户


sudo adduser lilei    --> su -l lilei  退出当前用户跟退出终端一样可以使用 exit 命令或者使用快捷键 Ctrl+d。

groups shiyanlou  自己属于哪些用户组

cat /etc/group | grep -E "shiyanlou|sudo"  group_name:password:GID:user_list

cat /etc/passwd 查看系统所有用户，uid 500之前为系统用户

sudo usermod -G sudo lilei  添加用户到用户组

删除用户是很简单的事：

$ sudo deluser lilei --remove-home

deluser USER
  remove a normal user from the system
  example: deluser mike

  --remove-home             remove the users home directory and mail spool
  --remove-all-files        remove all files owned by user
  --backup                  backup files before removing.
  --backup-to <DIR>         target directory for the backups.
                            Default is the current directory.
  --system                  only remove if system user

delgroup GROUP
deluser --group GROUP
  remove a group from the system
  example: deluser --group students

  --system                  only remove if system group
  --only-if-empty           only remove if no members left

deluser USER GROUP
  remove the user from a group
  example: deluser mike students

general options:
  --quiet | -q      don't give process information to stdout
  --help | -h       usage message
  --version | -v    version number and copyright
  --conf | -c FILE  use FILE as configuration file


  显示除了 '.'(当前目录)，'..' 上一级目录之外的所有包含隐藏文件（Linux 下以 '.' 开头的文件为隐藏文件）
$ ls -A


当然，你可以同时使用 '-A' 和 '-l' 参数：

$ ls -Al
查看某一个目录的完整属性，而不是显示目录里面的文件属性：

$ ls -dl <目录名>
显示所有文件大小，并以普通人类能看懂的方式呈现：
$ ls -AsSh
其中小 s 为显示文件大小，大 S 为按文件大小排序，若需要知道如何按其它方式排序，请使用“man”命令查询。


* 修改文件所有者   `sudo chown shiyanlou:nogroup iphone6` `chown -R liu /usr/meng`  
 [chown man](http://man.linuxde.net/chown)

* 修改权限：`$ chmod 700 iphone6``chmod go-rw iphone6`一般必须有sudo权限才能用这两个命令

目录


- 表示上一次所在目录，～ 通常表示当前用户的"home"目录

提示：在进行目录切换的过程中请多使用 Tab 键自动补全，可避免输入错误，连续按两次Tab可以显示全部候选结果

复制和删除目录  加-r参数

nl命令，添加行号并打印，这是个比cat -n更专业的行号打印命令
-b : 指定添加行号的方式，主要有两种：
    -b a:表示无论是否为空行，同样列出行号("cat -n"就是这种方式)
    -b t:只列出非空行的编号并列出（默认为这种方式）
-n : 设置行号的样式，主要有三种：
    -n ln:在行号字段最左端显示
    -n rn:在行号字段最右边显示，且不加 0
    -n rz:在行号字段最右边显示，且加 0
-w : 行号字段占用的位数(默认为 6 位)


更直接的只看一行， 加上-n参数，后面紧跟行数：

$ tail -n 1 /etc/passwd    -f 参数 实时刷新

查看文件类型   file /bin/ls

vimtutor  vim学习

环境变量就是作用域比自定义变量要大，如Shell 的环境变量作用于自身和它的子进程。在所有的 UNIX 和类 UNIX 系统中，每个进程都有其各自的环境变量设置，且默认情况下，当一个进程被创建时，处理创建过程中明确指定的话，它将继承其父进程的绝大部分环境设置。Shell 程序也作为一个进程运行在操作系统之上，而我们在 Shell中运行的大部分命令都将以 Shell 的子进程的方式运行。

命令  说明
set 显示当前 Shell 所有环境变量，包括其内建环境变量（与 Shell 外观等相关），用户自定义变量及导出的环境变量
env 显示与当前用户相关的环境变量，还可以让命令在指定环境中运行
export  显示从 Shell 中导出成环境变量的变量，也能通过它将自定义变量导出为环境变量

zsh  bash  启动一个新的子进程  exit  退出  变量的作用域：自定义，环境变量

查看是否4k对齐
sudo fdisk -lu


搜索文件


whereis   简单快速
whereis who

whereis只能搜索二进制文件(-b)，man帮助文件(-m)和源代码文件(-s)。

locate 快而全
locate /etc/sh  查找 /etc 下所有以 sh 开头的文件，它不只是在 etc 目录下查找并会自动递归子目录进行查找
locate /udo mkdir jvm
* 支持正则表达式  `locate --regexp aa`

通过"/var/lib/mlocate/mlocate.db"数据库查找，不过这个数据库也不是实时更新的，系统会使用定时任务每天自动执行updatedb命令更新一次，所以有时候你刚添加的文件，它可能会找不到，需要手动执行一次sudo updatedb命令（在我们的环境中必须先执行一次该命令）。
想只统计数目可以加上-c参数，-i参数可以忽略大小写进行查找，whereis 的-b,-m，-s同样可以是使用。

which小而精
which本身是 Shell 内建的一个命令，我们通常使用which来确定是否安装了某个指定的软件，因为它只从PATH环境变量指定的路径中去搜索命令：

$ which man

find

find应该是这几个命令中最强大的了，它不但可以通过文件类型、文件名进行查找而且可以根据文件的属性（如文件的时间戳，文件的权限等）进行搜索。

指定目录下搜索指定文件名的文件：

$ find /etc/ -name interfaces
注意 find 命令的路径是作为第一个参数的， 基本命令格式为
find [path] [option] [action]
与时间相关的命令参数：

参数	说明
-atime	最后访问时间
-ctime	创建时间
-mtime	最后修改时间
下面以-mtime参数举例：

-mtime n: n 为数字，表示为在n天之前的”一天之内“修改过的文件
-mtime +n: 列出在n天之前（不包含n天本身）被修改过的文件
-mtime -n: 列出在n天之前（包含n天本身）被修改过的文件
newer file: file为一个已存在的文件，列出比file还要新的文件名

列出 home 目录中，当天（24 小时之内）有改动的文件：

$ find ~ -mtime 0
列出用户家目录下比Code文件夹新的文件：

$ find ~ -newer /home/shiyanlou/Code

* 通过文件类型 `find . -type f` 文件类型 `find . -type d` 文件夹
* 通过正则匹配 `find . -regex ".*\(\.txt\|\.pdf\)$"` `-iregex 忽略大小写`
* 限定路径层级 `find . -maxdepth 1 -iname '*mongo*'`

> [find 文档](http://man.linuxde.net/find "find")

文件压缩

zip -r 递归打包 -o 输出文件，其后紧跟打包输出文件名 -q安静模式 -【1-9】压缩级别，默认为最高9  -x 排除文件不压缩，只能使用绝对路径，否则不起作用。
zip -r -9 -q -o shiyanlou_9.zip /home/shiyanlou -x ~/*.zip
-e 加密 -l参数将LF转换为CR+LF 兼容windows
du命令查看打包后文件的大小  du -h -d 0 *.zip ~ | sort
-d, --max-depth（所查看文件的深度）

unzip -d 目标目录 -l 不解压只查看压缩包内容 -O（大写） 指定编码
unzip -q shiyanlou.zip -d ziptest
unzip -l shiyanlou.zip
unzip -O GBK 中文压缩文件.zip

rar a 添加一个目录  d删除某个文件 l 查看不解压
rar a shiyanlou.rar .
rar d shiyanlou.rar .zshrc
rar l shiyanlou.rar
unrar x 全路经解压  e 去掉路径解压
unrar x shiyanlou.rar
mkdir tmp
$ unrar e shiyanlou.rar tmp/

tar -c 创建一个tar包 -f指定创建的文件名 -v可视化输出打包文件（默认去掉前面的绝对路径/使用-P保留绝对路径） 解包一个文件(-x参数)到指定路径的已存在目录(-C参数)：  只查看不解包文件-t参数, r 追加文档到tar文件尾部.
保留文件属性和跟随链接（符号链接或软链接），有时候我们使用tar备份文件当你在其他主机还原时希望保留文件的属性(-p参数)和备份链接指向的源文件而不是链接本身(-h参数)：

tar -cf shiyanlou.tar ~
mkdir tardir
$ tar -xf shiyanlou.tar -C tardir
tar -tf shiyanlou.tar
tar -cphf etc.tar /etc
tar -rvf shiyanlou.tar file_extra.txt
我们只需要在创建 tar 文件的基础上添加-z参数，使用gzip来压缩/解压缩文件：

压缩文件格式	参数
*.tar.gz	-z
*.tar.xz	-J
*tar.bz2	-j

tar -czf shiyanlou.tar.gz ~
tar -xzf shiyanlou.tar.gz

查看磁盘容量  df -h
查看目录容量  du -h -d（目录深度0-一级 1-二级）

1.创建虚拟磁盘
dd命令用于转换和复制文件，，dd 也可以读取自和/或写入到这些文件。这样，dd也可以用在备份硬件的引导扇区、获取一定数量的随机数据或者空数据等任务中。dd程序也可以在复制时处理数据，例如转换字节序、或在 ASCII 与 EBCDIC 编码间互换。
dd默认从标准输入中读取，并写入到标准输出中，但可以用选项if（input file，输入文件）和of（output file，输出文件）改变。
dd if=/dev/stdin of=/dev/stdout bs=10 count=1
上述命令从标准输入设备读入用户输入（缺省值，所以可省略）然后输出到 test 文件，bs（block size）用于指定块大小（缺省单位为 Byte，也可为其指定如'K'，'M'，'G'等单位），count用于指定块数量。如上图所示，我指定只读取总共 10 个字节的数据，当我输入了“hello shiyanlou”之后加上空格回车总共 16 个字节（一个英文字符占一个字节）内容，显然超过了设定大小。使用和du和cat命令看到的写入完成文件实际内容确实只有 10 个字节（那个黑底百分号表示这里没有换行符）,而其他的多余输入将被截取并保留在标准输入。
将输出的英文字符转换为大写再写入文件：

$ dd if=/dev/stdin of=test bs=10 count=1 conv=ucase


使用mkfs格式化磁盘
输入 mkfs 然后按下Tab键，你可以看到很多个以 mkfs 为前缀的命令，这些不同的后缀其实就是表示着不同的文件系统，可以用 mkfs 格式化成的文件系统：
 mkfs.ext4 virtual.img

如果你想想知道 Linux 支持哪些文件系统你可以输入ls -l /lib/modules/$(uname -r)/kernel/fs（我们的环境中无法查看）查看。


使用 mount 命令挂载磁盘到目录树

用户在 Linux/UNIX 的机器上打开一个文件以前，包含该文件的文件系统必须先进行挂载的动作，此时用户要对该文件系统执行 mount 的指令以进行挂载。通常是使用在 USB 或其他可移除存储设备上，而根目录则需要始终保持挂载的状态。又因为 Linux/UNIX 文件系统可以对应一个文件而不一定要是硬件设备，所以可以挂载一个包含文件系统的文件到目录树。
使用mount来查看下主机已经挂载的文件系统： sudo mount

输出的结果中每一行表示一个设备或虚拟设备,每一行最前面是设备名，然后是 on 后面是挂载点，type 后面表示文件系统类型，再后面是挂载选项（比如可以在挂载时设定以只读方式挂载等等）。

mount [-o [操作选项]] [-t 文件系统类型] [-w|--rw|--ro] [文件系统源] [挂载点]
 mount -o loop -t ext4 virtual.img /mnt  

* 也可以省略挂载类型，很多时候 mount 会自动识别

* 以只读方式挂载
$ mount -o loop --ro virtual.img /mnt
或者mount -o loop,ro virtual.img /mnt

使用 umount 命令卸载已挂载磁盘

* 命令格式 sudo umount 已挂载设备名或者挂载点，如：
$ sudo umount /mnt

在类 UNIX 系统中，/dev/loop（或称vnd （vnode disk）、lofi（循环文件接口））是一种伪设备，这种设备使得文件可以如同块设备一般被访问。

在使用之前，循环设备必须与现存文件系统上的文件相关联。这种关联将提供给用户一个应用程序接口，接口将允许文件视为块特殊文件（参见设备文件系统）使用。因此，如果文件中包含一个完整的文件系统，那么这个文件就能如同磁盘设备一般被挂载。

这种设备文件经常被用于光盘或是磁盘镜像。通过循环挂载来挂载包含文件系统的文件，便使处在这个文件系统中的文件得以被访问。这些文件将出现在挂载点目录。如果挂载目录中本身有文件，这些文件在挂载后将被禁止使用。

使用 fdisk 为磁盘分区
查看硬盘分区表信息
$ sudo fdisk -l



3.apt-get

apt-get使用各用于处理apt包的公用程序集，我们可以用它来在线安装、卸载和升级软件包等，下面列出一些apt-get包含的常用的一些工具：

工具	说明
install	其后加上软件包名，用于安装一个软件包
update	从软件源镜像服务器上下载/更新用于更新本地软件源的软件包列表
upgrade	升级本地可更新的全部软件包，但存在依赖问题时将不会升级，通常会在更新之前执行一次update
dist-upgrade	解决依赖关系并升级(存在一定危险性)
remove	移除已安装的软件包，包括与被移除软件包有依赖关系的软件包，但不包含软件包的配置文件
autoremove	移除之前被其他软件包依赖，但现在不再被使用的软件包
purge	与remove相同，但会完全移除软件包，包含其配置文件
clean	移除下载到本地的已经安装的软件包，默认保存在/var/cache/apt/archives/
autoclean	移除已安装的软件的旧版本软件包
下面是一些apt-get常用的参数：

参数	说明
-y	自动回应是否安装软件包的选项，在一些自动化安装脚本中使用这个参数将十分有用
-s	模拟安装
-q	静默安装方式，指定多个q或者-q=#,#表示数字，用于设定静默级别，这在你不想要在安装软件包时屏幕输出过多时很有用
-f	修复损坏的依赖关系
-d	只下载不安装
--reinstall	重新安装已经安装但可能存在问题的软件包
--install-suggests	同时安装APT给出的建议安装的软件包


你还应该掌握的是如何重新安装软件包。 很多时候我们需要重新安装一个软件包，比如你的系统被破坏，或者一些错误的配置导致软件无法正常工作。
sudo apt-get --reinstall install w3m

* 更新软件源
$ sudo apt-get update

* 升级没有依赖问题的软件包
$ sudo apt-get upgrade

* 升级并解决依赖关系
$ sudo apt-get dist-upgrade

* 不保留配置文件的移除
$ sudo apt-get purge w3m
 或者 sudo apt-get --purge remove

* 移除不再需要的被依赖的软件包
$ sudo apt-get autoremove

* apt-cache 命令则是针对本地数据进行相关操作的工具，search 顾名思义在本地的数据库中寻找有关 softname1 softname2 …… 相关软件的信息。
sudo apt-cache search softname1 softname2 softname3……


* dpkg常用参数介绍：
参数	说明
-i	安装指定deb包
-R	后面加上目录名，用于安装该目录下的所有deb安装包
-r	remove，移除某个已安装的软件包
-I	显示deb包文件的信息
-s	显示已安装软件的信息
-S	搜索已安装的软件包
-L	显示已安装软件包的目录信息

apt-get加上-d参数只下载不安装，下载emacs编辑器的deb包，下载完成后，我们可以查看/var/cache/apt/archives/目录下的内容下载的所有deb包
sudo apt-get -d install emacs
安装之前参看deb包的信息
$ sudo dpkg -I emacs24_24.3+1-4ubuntu1_amd64.deb
这个包还额外依赖了一些软件包，这意味着，如果主机目前没有这些被依赖的软件包，直接使用dpkg安装可能会存在一些问题，因为dpkg并不能为你解决依赖关系。
使用dpkg安装
$ sudo dpkg -i emacs24_24.3+1-4ubuntu1_amd64.deb
我们将如何解决这个错误了，这就要用到apt-get了，使用它的-f参数了，修复依赖关系的安装
$ sudo apt-get -f install

使用dpkg -L查看deb包目录信息
$ sudo dpkg -L emacs24
`sudo dpkg -l | grep soft_name` 搜索系统中安装的指定软件

linux 查看硬件信息
lscpu 查看cpu统计信息 或者 cat /proc/cpuinfo 查看每个cpu信息，如cpu型号，主频

查看概要内存情况 free -h 查看内存详细使用情况 cat /proc/meminfo

查看内存硬件（主板槽位信息）dmidecode -t memeory
* `getconf LONG_BIT` 查看操作系统位数  
`lsb_release -a` 查看当前系统版本信息
`uname -a` 查看系统内核信息

## jdk install
我们把JDK安装到这个路径：/usr/lib/jvm
如果没有这个目录（第一次当然没有），我们就新建一个目录

cd /usr/lib
sudo mkdir jvm
建立好了以后，我们来到刚才下载好的压缩包的目录，解压到我们刚才新建的文件夹里面去，并且修改好名字方便我们管理

sudo tar zxvf ./jdk-7-linux-i586.tar.gz  -C /usr/lib/jvm
cd /usr/lib/jvm
sudo mv jdk1.7.0_05/ jdk7

 * 配置环境变量

gedit /etx/profile

在打开的文件的末尾添加

export JAVA_HOME=/usr/lib/jvm/jdk7


export JRE_HOME=${JAVA_HOME}/jre

export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib:$CLASSPATH


export PATH=${JAVA_HOME}/bin:$PATH

保存退出，然后输入下面的命令来使之生效

source ~/.bashrc

 * 配置默认JDK

由于一些Linux的发行版中已经存在默认的JDK，如OpenJDK等。所以为了使得我们刚才安装好的JDK版本能成为默认的JDK版本，我们还要进行下面的配置。
执行下面的命令：

sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk7/bin/java 300


sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk7/bin/javac 300

 注意：如果以上两个命令出现找不到路径问题，只要重启一下计算机在重复上面两行代码就OK了。

执行下面的代码可以看到当前各种JDK版本和配置：

sudo update-alternatives --config java

 * 测试

打开一个终端，输入下面命令：

java -version

显示结果：

java version "1.7.0_05"
Java(TM) SE Runtime Environment (build 1.7.0_05-b05)


Java HotSpot(TM) Server VM (build 23.1-b03, mixed mode)

这表示java命令已经可以运行了。
### uniq
> 管道过滤唯一字符  

### ln [创建链接：官方文档](http://man.linuxde.net/ln)
### 查看当前服务和监听端口  `sudo netstat -atunlp | grep mongo`
### ubuntu 修改主机名
```
//查看主机名
topcom@master:~$ hostname
master
//永久修改
vim /etc/hostname
```
### 复制符号连接，保留文件属性，递归操作 -dpr
`cp -av source dest`

### 查看硬链接ln关联的所有文件及路径  
```bash
ls -i  myInfo.txt
3814056 myInfo.txt
find / -inum 3814056 
/home/homer/me/myInfo.txt
/home/homer/me/.me/myInfo.txt_ln
/home/homer/bin/myInfo.txt
```

### Bash Shell 中文件名不同颜色的含义

> 蓝色－目录
绿色－可执行文件
红色－压缩文件
浅蓝色－链接文件
灰色－其他文件
紫色－图形文件
黄色－设备文件
棕色－FIFO文件（先进先出，命令管道）
