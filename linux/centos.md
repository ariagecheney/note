## 查看发行版信息
`cat /etc/issue`

## [理解Linux系统/etc/init.d目录和/etc/rc.local脚本](http://blog.csdn.net/acs713/article/details/7322082)

## 关机or重启
`shutdown -h now`  
`sudo shutdown -r +1 "off"`
## lrzsz
* rz -be
* sz
`yum install lrzsz`

## ssh 远程登陆超时自动退出 时间配置 
`vim /etc/profile`
```shell
# 单位毫秒，0 
TMOUT=0
export TMOUT
```
`source /etc/profile`
## 测试ip和端口是否通
* wget ip:port
* ssh -v -p port ip
## 设置别名 /etc/bashrc
```shell
alias cd1="cd ../"
alias cd2="cd ../../"
alias cd4="cd ../../../../"
alias cd3="cd ../../../"
alias grep="grep --color"
alias egrep="egrep --color"
```

## 配置dns
vi /etc/resolv.conf 文件内容   
nameserver 8.8.8.8  
 nameserver 8.8.4.4   
 search localdomain  
//重启网络  
service network restart 或 /etc/init.d/network restart 

* 查看运行级别用：runlevel  /etc/inittab  startx或者xinit 
0：停止系统运行。init 0〈回车〉相当于 halt〈回车〉。
6：重启系统。init 6〈回车〉相当于 reboot〈回车〉。

* hostname -i 查看本地ip

使用命令：chkconfig sshd on 设置为开机启动
使用命令：chkconfig --list |grep sshd查看设置结果，
service sshd start
## 查看外网 ip
* `curl ifconfig.me`  
* `curl cip.cc`
## 查看位数
file /sbin/init 或者 file /bin/ls   
uname -a:
## 安装ftp客户端
`rpm -Uvh http://mirror.centos.org/centos/6/os/x86_64/Packages/ftp-0.17-54.el6.x86_64.rpm`
* ftp 登陆命令  
`ftp ip port`
## rpm
```shell
rpm -qa | grep mysql　　// 这个命令就会查看该操作系统上是否已经安装了mysql数据库

rpm -ivh --force --nodeps package.rpm
rpm -Uvh pac.rpm 升级
rpm -ql 包名 安装列表

rpm -qpi package.rpm 查询软件描述信息
rpm -qpl package.rpm 列出软件文件信息

rpm -e mysql　　// 普通删除模式
rpm -e --nodeps mysql　　// 强力删除模式，如果使用上面命令删除时，提示有依赖的其它文件，则用该命令可以对其进行强力删除

which mysql 找出可执行文件

rpm -qilf `which 程序名` 安装来源

```

## yum
```shell

yum的命令形式一般是如下：yum [options] [command] [package ...]
其中的[options]是可选的，选项包括-h（帮助），-y（当安装过程提示选择全部为"yes"），-q（不显示安装的过程）等等。[command]为所要进行的操作，[package ...]是操作的对象。

概括了部分常用的命令包括：

自动搜索最快镜像插件：   yum install yum-fastestmirror
安装yum图形窗口插件：    yum install yumex
查看可能批量安装的列表： yum grouplist

1 安装
yum install 全部安装
yum install package1 安装指定的安装包package1
yum groupinsall group1 安装程序组group1

2 更新和升级
yum update 全部更新
yum update package1 更新指定程序包package1
yum check-update 检查可更新的程序
yum upgrade package1 升级指定程序包package1
yum groupupdate group1 升级程序组group1

3 查找和显示
yum info package1 显示安装包信息package1
yum list 显示所有已经安装和可以安装的程序包
yum list updates 列出所有可更新的软件包
yum list installed 列出所有已安装的软件包
yum list extras 列出所有已安装但不在 Yum Repository 內的软件包
yum list package1 显示指定程序包安装情况package1
yum groupinfo group1 显示程序组group1信息
yum search string 根据关键字string查找安装包

4 删除程序
yum remove &#124; erase package1 删除程序包package1
yum groupremove group1 删除程序组group1
yum deplist package1 查看程序package1依赖情况

5 清除缓存
yum clean packages 清除缓存目录/var/cache/yum下的软件包
yum clean headers 清除缓存目录下的 headers
yum clean oldheaders 清除缓存目录下旧的 headers
yum clean, yum clean all (= yum clean packages; yum clean oldheaders) 清除缓存目录下的软件包及旧的headers
```

## scp
* 命令基本格式： scp [可选参数] file_source file_target 
* ssh -p port user@host
* scp -r /home/space/music/ root@www.cumt.edu.cn:/home/root/others/   
 上面 命令 将 本地 music 目录 复制 到 远程 others 目录下，即复制后有 远程 有 ../others/music/ 目录 
* 可选参数  
```shell
-v 和大多数 linux 命令中的 -v 意思一样 , 用来显示进度 . 可以用来查看连接 , 认证 , 或是配置错误 . 

-C 使能压缩选项 . 

-P 选择端口 . 注意 -p 已经被 rcp 使用 . 

-4 强行使用 IPV4 地址 . 

-6 强行使用 IPV6 地址 .

-2 强制scp命令使用协议ssh2
```

* 注意两点：  
1. 如果远程服务器防火墙有特殊限制，scp便要走特殊端口，具体用什么端口视情况而定，命令格式如下：  
`scp -P 4588 remote@www.abc.com:/usr/local/sin.sh /home/administrator`   
2. 使用scp要注意所使用的用户是否具有可读取远程服务器相应文件的权限。

## msyql yum install
* 查看msyql 运行状态  
service mysqld status  
ps aux |grep mysqld  

* 启动 重启  
service mysqld start  
service mysqld restart  
* 是不是开机启动   
chkconfig --list |grep mysqld
* chkconfig --list如果列表中没有mysqld这个，需要先用这个命令添加：
chkconfig add mysqld  
* 设置开机启动   
 chkconfig mysqld on  
*  为root账号设置密码   
/usr/bin/mysqladmin -u root password 'new-password'  　　

* 登录  
mysql -u root -p  

* 配置  
/etc/my.cnf  

* 查看端口  
netstat -anp   
## mysql 
* [linux mysql 更改MySQL数据库目录位置](http://blog.csdn.net/qq_36040184/article/details/53889856)
* whereis   
初始化 MySQL 数据库： WARNING: The host 'AQWSnginx' could not be looked up with resolveip.
This probably means that your libc libraries are not 100 % compatible
with this binary MySQL version. The MySQL daemon, mysqld, should work
normally with the exception that host name resolving will not work.
This means that you should use IP addresses instead of hostnames
when specifying MySQL privileges !
Installing MySQL system tables...
OK
Filling help tables...
OK

To start mysqld at boot time you have to copy
support-files/mysql.server to the right place for your system

PLEASE REMEMBER TO SET A PASSWORD FOR THE MySQL root USER !
To do so, start the server, then issue the following commands:

/usr/bin/mysqladmin -u root password 'new-password'
/usr/bin/mysqladmin -u root -h AQWSnginx password 'new-password'

Alternatively you can run:
/usr/bin/mysql_secure_installation

which will also give you the option of removing the test
databases and anonymous user created by default.  This is
strongly recommended for production servers.

See the manual for more instructions.

You can start the MySQL daemon with:
cd /usr ; /usr/bin/mysqld_safe &

You can test the MySQL daemon with mysql-test-run.pl
cd /usr/mysql-test ; perl mysql-test-run.pl

Please report any problems with the /usr/bin/mysqlbug script!

                      