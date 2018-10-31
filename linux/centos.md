## 查看发行版信息
`cat /etc/issue`  
`cat /etc/centos-release `  
or  
`lsb_release -a` 查看当前系统版本信息  
## linux 查看硬件信息
* `cat /proc/cpuinfo or lscpu` 查看cpu统计信息 
```
socket：物理CPU的插槽
Core per Socket：每一个插槽对应的物理CPU上有多少个核
Thread per Core：每个核上有多少个线程

看个图：（几核几线程就是指有多少个“Core per Socket”多少个“Thread per Core”,当后者比前者多时，
说明启用了超线程技术）
```
* `free -h` 查看概要内存情况  
* `cat /proc/meminfo` 查看内存详细使用情况 
* `dmidecode -t memory` 查看内存硬件（主板槽位信息）
* `getconf LONG_BIT` 查看操作系统位数  
* `uname -a` 查看系统内核信息
* `df -ahT` 查看磁盘容量  
* `du -sh dir` Show the size of a single folder, in human readable units:
## [理解Linux系统/etc/init.d目录和/etc/rc.local脚本](http://blog.csdn.net/acs713/article/details/7322082)

## 中文语言
* echo $LANG 
查看当前语言
* locale -a
查看是否安装中文语言包（zh_CN.UTF-8）  
* 安装中文语言包  
`yum groupinstall chinese-support`  

* `vim  /etc/sysconfig/i18n` 配置文件   
设置 `LANG="zh_CN.UTF-8"`
重启
* 临时设置 LANG 环境变量也可，即时生效
## 关机or重启
`shutdown -h now`  
`sudo shutdown -r +1 "off"`
## lrzsz
* rz -bey ：上传
* sz 文件  ：下载
`yum install lrzsz`

## ssh 远程登陆超时自动退出 时间配置 
`vim /etc/profile`
```shell
# 单位毫秒，0 
TMOUT=0
export TMOUT
```
`source /etc/profile`
## CentOS ping: unknown host 解决方法

1. 配置dns   
您必须是管理员root或者具有管理员权限   
sudo vim /etc/resolv.conf   
nameserver 8.8.8.8    
nameserver 8.8.4.4
nameserver 1.1.1.1  
nameserver 1.0.0.1                                
nameserver 223.5.5.5  
nameserver 223.6.6.6  
* 保存退出，然后使用dig 验证:  
dig www.taobao.com +short  
若出现结果则表示正常。  
2. 确保路由表正常  
netstat -rn (route -n 获取网关)
如果未设置, 则通过如下方式增加网关:
route add default gw 192.168.1.1
3. 确保可用dns解析
grep hosts /etc/nsswitch.conf 
> hosts:      files dns  
4. 重启网络  
service network restart 或 /etc/init.d/network restart 

## 查看运行级别用：runlevel  /etc/inittab  startx或者xinit 
0：停止系统运行。init 0〈回车〉相当于 halt〈回车〉。
6：重启系统。init 6〈回车〉相当于 reboot〈回车〉。

* hostname -i 查看本地ip

使用命令：chkconfig sshd on 设置为开机启动
使用命令：chkconfig --list |grep sshd查看设置结果，
service sshd start

## 查看位数
file /sbin/init 或者 file /bin/ls   
uname -a:
## 安装ftp客户端
`rpm -Uvh http://mirror.centos.org/centos/6/os/x86_64/Packages/ftp-0.17-54.el6.x86_64.rpm`
* ftp 登陆命令  
`ftp ip port`

## centos 编译安装git
```sh
// 卸载自带的git
yum remove git
// 依赖
yum install curl-devel expat-devel gettext-devel \
  openssl-devel zlib-devel
下载最新版git
wget https://www.kernel.org/pub/software/scm/git/git-2.17.0.tar.gz
解压
tar zxvf git-2.17.0.tar.gz
cd git-2.17.0
编译安装
make prefix=/usr/local all
sudo make prefix=/usr/local install
修改环境变量
sudo vim /etc/profile
在最后一行添加
export PATH=/usr/local/git/bin:$PATH
保存后使其立即生效
source /etc/profile
查看是否安装成功
git --version
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


2018-10-12T07:45:06.692094Z 0 [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use --explicit_defaults_for_timestamp server option (see documentation for more details).
2018-10-12T07:45:06.692194Z 0 [Warning] 'NO_ZERO_DATE', 'NO_ZERO_IN_DATE' and 'ERROR_FOR_DIVISION_BY_ZERO' sql modes should be used with strict mode. They will be merged with strict mode in a future release.
2018-10-12T07:45:06.692203Z 0 [Warning] 'NO_AUTO_CREATE_USER' sql mode was not set.
2018-10-12T07:45:07.838275Z 0 [Warning] InnoDB: New log files created, LSN=45790
2018-10-12T07:45:08.101388Z 0 [Warning] InnoDB: Creating foreign key constraint system tables.
2018-10-12T07:45:08.241585Z 0 [Warning] No existing UUID has been found, so we assume that this is the first time that this server has been started. Generating a new UUID: bda6f64e-cdf2-11e8-ba63-a0369fc0ee22.
2018-10-12T07:45:08.271693Z 0 [Warning] Gtid table is not ready to be used. Table 'mysql.gtid_executed' cannot be opened.
2018-10-12T07:45:08.272820Z 1 [Note] A temporary password is generated for root@localhost: EtmkBtbL5>Mz


EtmkBtbL5>Mz

## [rar/unrar](https://www.rarlab.com/download.htm)
* rar软件不需要安装，直接解压到/usr/local下，以下操作需要有root权限。

* 然后执行以下命令  
`ln -s /usr/local/rar/rar /usr/local/bin/rar;ln -s /usr/local/rar/unrar /usr/local/bin/unrar` 
`

# centos 7
* yum upgrade && yum install net-tools

                      