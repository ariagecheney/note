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

## 查看位数
file /sbin/init 或者 file /bin/ls   
uname -a:

## rpm
```shell
rpm -qa | grep mysql　　// 这个命令就会查看该操作系统上是否已经安装了mysql数据库

rpm -ivh --force --nodeps package.rpm

rpm -ql 包名 安装列表

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

## ssh
* ssh -p port user@host
* scp -P10022 CommGuard.sql evercloud@202.107.70.9:/home/evercloud/
命令基本格式： 
       scp [可选参数] file_source file_target 

====== 
从 本地 复制到 远程 
====== 
* 复制文件： 
        * 命令格式： 
                scp local_file remote_username@remote_ip:remote_folder 
                或者 
                scp local_file remote_username@remote_ip:remote_file 
                或者 
                scp local_file remote_ip:remote_folder 
                或者 
                scp local_file remote_ip:remote_file 

                第1,2个指定了用户名，命令执行后需要再输入密码，第1个仅指定了远程的目录，文件名字不变，第2个指定了文件名； 
                第3,4个没有指定用户名，命令执行后需要输入用户名和密码，第3个仅指定了远程的目录，文件名字不变，第4个指定了文件名； 
        * 例子： 
                scp /home/space/music/1.mp3 root@www.cumt.edu.cn:/home/root/others/music 
                scp /home/space/music/1.mp3 root@www.cumt.edu.cn:/home/root/others/music/001.mp3 
                scp /home/space/music/1.mp3 www.cumt.edu.cn:/home/root/others/music 
                scp /home/space/music/1.mp3 www.cumt.edu.cn:/home/root/others/music/001.mp3 

* 复制目录： 
        * 命令格式： 
                scp -r local_folder remote_username@remote_ip:remote_folder 
                或者 
                scp -r local_folder remote_ip:remote_folder 

                第1个指定了用户名，命令执行后需要再输入密码； 
                第2个没有指定用户名，命令执行后需要输入用户名和密码； 
        * 例子： 
                scp -r /home/space/music/ root@www.cumt.edu.cn:/home/root/others/ 
                scp -r /home/space/music/ www.cumt.edu.cn:/home/root/others/ 

                上面 命令 将 本地 music 目录 复制 到 远程 others 目录下，即复制后有 远程 有 ../others/music/ 目录 


====== 
从 远程 复制到 本地 
====== 
从 远程 复制到 本地，只要将 从 本地 复制到 远程 的命令 的 后2个参数 调换顺序 即可； 

例如： 
        scp root@www.cumt.edu.cn:/home/root/others/music /home/space/music/1.mp3 
        scp -r www.cumt.edu.cn:/home/root/others/ /home/space/music/

最简单的应用如下 : 

scp 本地用户名 @IP 地址 : 文件名 1 远程用户名 @IP 地址 : 文件名 2 

[ 本地用户名 @IP 地址 :] 可以不输入 , 可能需要输入远程用户名所对应的密码 . 

可能有用的几个参数 : 

-v 和大多数 linux 命令中的 -v 意思一样 , 用来显示进度 . 可以用来查看连接 , 认证 , 或是配置错误 . 

-C 使能压缩选项 . 

-P 选择端口 . 注意 -p 已经被 rcp 使用 . 

-4 强行使用 IPV4 地址 . 

-6 强行使用 IPV6 地址 .

-2 强制scp命令使用协议ssh2
 

注意两点：
1.如果远程服务器防火墙有特殊限制，scp便要走特殊端口，具体用什么端口视情况而定，命令格式如下：
#scp -P 4588 remote@www.abc.com:/usr/local/sin.sh /home/administrator
2.使用scp要注意所使用的用户是否具有可读取远程服务器相应文件的权限。

## msyql yum install
* 查看msyql 运行状态  
service mysqld status  
ps aux |grep mysqld  

* 启动 重启  
service mysqld start  
service mysqld restart  
* 是不是开机启动   
chkconfig --list |grep mysqld  
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

                      