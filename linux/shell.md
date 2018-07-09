# shell 脚本学习
## 命令在Linux中的执行分为4个步骤
* 个人猜测首先进行：当参数中有通配符时，shell 首先进行参数的通配符路径扩展，然后再组装新的命令
* 第1步：判断用户是否以绝对路径或相对路径的方式输入命令（如/bin/ls），如果是的话则直接执行。
* 第2步：Linux系统检查用户输入的命令是否为“别名命令”，即用一个自定义的命令名称来替换原本的命令名称。可以用alias命令来创建一个属于自己的命令别名，格式为“alias 别名=命令”。若要取消一个命令别名，则是用unalias命令，格式为“unalias 别名”。
* 第3步：Bash解释器判断用户输入的是内部命令还是外部命令。内部命令是解释器内部的指令，会被直接执行；而用户在绝大部分时间输入的是外部命令，这些命令交由步骤4继续处理。可以使用“type 命令名称”来判断用户输入的命令是内部命令还是外部命令。
* 第4步：系统在多个路径中查找用户输入的命令文件，而定义这些路径的变量叫作PATH，可以简单地把它理解成是“解释器的小助手”，作用是告诉Bash解释器待执行的命令可能存放的位置，然后Bash解释器就会乖乖地在这些位置中逐个查找。PATH是由多个路径值组成的变量，每个路径值之间用冒号间隔，对这些路径的增加和删除操作将影响到Bash解释器对Linux命令的查找。
* env命令来查看到Linux系统中所有的环境变量
## [前台任务，后台任务，守护进程](http://www.ruanyifeng.com/blog/2016/02/linux-daemon.html)
* "后台任务"与"前台任务"的本质区别只有一个：是否继承标准输入。所以，执行后台任务的同时，用户还可以输入其他命令。

## systemd
### [Systemd 入门教程：命令篇](http://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-commands.html)
## `systemctl--version`
* 表1-4   systemctl管理服务的启动、重启、停止、重载、查看状态等常用命令
```sh
System V init命令（RHEL 6系统）	systemctl命令（RHEL 7系统）	作用

service foo start	systemctl start foo.service	启动服务  
service foo restart	systemctl restart foo.service	重启服务  
service foo stop	systemctl stop foo.service	停止服务  
service foo reload	systemctl reload foo.service	重新加载配置文件（不终止服务）  
service foo status	systemctl status foo.service	查看服务状态 
``` 
* 表1-5    systemctl设置服务开机启动、不启动、查看各级别下服务启动状态等常用命令
```sh
System V init命令（RHEL 6系统）	systemctl命令（RHEL 7系统）	作用  

chkconfig foo on	systemctl enable foo.service	开机自动启动
chkconfig foo off	systemctl disable foo.service	开机不自动启动
chkconfig foo	systemctl is-enabled foo.service	查看特定服务是否为开机自启动
chkconfig --list	systemctl list-unit-files --type=service	查看各个级别下服务的启动与禁用情况
```
## [Shell 中的变量作用域](http://harttle.land/2017/04/03/shell-variable-scope.html)

## [Shell 变量引用和字符串处理](https://liam0205.me/2016/11/08/Shell-variable-reference-and-string-cut-off/)
## [shell 基本语法](https://www.linuxprobe.com/chapter-04.html#423)

## [shopt](http://man.linuxde.net/shopt)

## [curl命令的基本使用](https://www.cnblogs.com/yinzhengjie/p/7719804.html)
## sed
Stream Editor文本流编辑，sed是一个“非交互式的”面向字符流的编辑器。能同时处理多个文件多行的内容，可以不对原文件改动，把整个文件输入到屏幕,可以把只匹配到模式的内容输入到屏幕上。还可以对原文件改动，但是不会再屏幕上返回结果。

### sed命令的语法格式：

sed的命令格式： sed [option] 'sed command'filename

sed的脚本格式：sed [option] -f 'sed script'filename

### sed命令的选项(option)：

-n ：只打印模式匹配的行

-e ：直接在命令行模式上进行sed动作编辑，此为默认选项，多个选项，每个操作用 -e 参数

-f ：将sed的动作写在一个文件内，用–f filename 执行filename内的sed动作

-r ：支持扩展表达式

-i ：直接修改文件内容

## sed在文件中查询文本的方式：

#### 1. 使用行号，可以是一个简单数字，或是一个行号范围

x x为行号

x,y 表示行号从x到y

/pattern/ 查询包含模式的行

/pattern/pattern/ **按顺序**，查找匹配模式的行

/pattern/,x **按顺序**在给定行号上查询包含模式的行

x,/pattern/  **按顺序**通过行号和模式查询匹配的行

x,y! **取反**查询不包含指定行号x和y的行

例子：  
`sed -n '/wf/,/jj/p' ori.txt`

#### 2. 使用正则表达式、扩展正则表达式(必须结合-r选项)

### sed的编辑命令(sed command)
* 对文件的操作无非就是”增删改查“

## [expr](http://man.linuxde.net/expr)
```sh
expr的常用运算符：

加法运算：+
减法运算：-
乘法运算：\*
除法运算：/
求摸（取余）运算：%

result=`expr 2 \* 3`
result=$(expr $no1 + 5) 或 $(($no1 * 3))
```

## curl
* https://curl.haxx.se/download/
* `wget https://curl.haxx.se/download/curl-7.53.1.tar.gz`
* 使用
```sh
curl
 Transfers data from or to a server.
 Supports most protocols, including HTTP, FTP, and POP3.

 Download the contents of an URL to a file:
 curl http://example.com -o filename

 Download a file, saving the output under the filename indicated by the URL:
 curl -O http://example.com/filename

 Download a file, following [L]ocation redirects, and automatically [C]ontinuing (resuming) a previous file transfer:
 curl -O -L -C - http://example.com/filename

  send params of get request 
  curl "http://ip:port/aa/bb/c?key1=value1&key2=value2"

 Send form-encoded data (POST request of type `application/x-www-form-urlencoded`):
 curl -d 'name=bob' http://example.com/form

 Send a request with an extra header, using a custom HTTP method:
 curl -H 'X-My-Header: 123' -X PUT http://example.com

 Send data in JSON format, specifying the appropriate content-type header:
 curl -d '{"name":"bob"}' -H 'Content-Type: application/json' http://example.com/users/1234

 Pass a user name and password for server authentication:
 curl -u myusername:mypassword http://example.com

 Pass client certificate and key for a resource, skipping certificate validation:
 curl --cert client.pem --key key.pem --insecure https://example.com

curl -fsSL:
-f fail
-s silently
-S --show-error
-L -L/--location


```
* [curl网站开发 by ruanyf](http://www.ruanyifeng.com/blog/2011/09/curl.html)

## gpg

*  Debian / Ubuntu 环境  
sudo apt-get install gnupg

* Fedora 环境  
yum install gnupg

gpg --help

* GnuPG1 与 GnuPG2 、GnuPG2.1 区别  

GnuPG 1  
gpg 仍然保留用作嵌入式和服务器端使用，这个更少依赖以及更小的二进制文件。 来自 man gpg :

This is the standalone version of gpg. For desktop use you should consider using gpg2.

GnuPG 2  
gpg2 是 gpg 的重新设计版本—— 但是更多的是在内部级别的更改。较新的版本分为多个模块，例如，有可用于 X.509 的模块

来自 man gpg2

In contrast to the standalone version gpg, which is more suited for server and embedded platforms, this version is commonly installed under the name gpg2 and more targeted to the desktop as it requires several other modules to be installed.

GnuPG 2.1  
GnuPG 2.1是一个重要的变化，它将以前分开的公钥和私钥（pubring.gpg vs secring.gpg）组合到公钥匙中。这是以兼容的方式实现的，所以当GnuPG 2.1集成了私有密钥环时，您仍然可以使用GnuPG 1，但私有密钥的更改不会出现在相应的其他实现中。

[…]允许与GnuPG 2.1共同存在较旧的GnuPG版本。 然而，使用新gpg的私钥的任何更改在使用2.1版本的GnuPG之前都不会出现，反之亦然。
## apt-get
* 配置文件  
/etc/apt/sources.list
* [debian系linux，更换apt-get官方源为国内源](https://blog.csdn.net/yjk13703623757/article/details/78943345/)
* 添加第三方源  
`apt-get install software-properties-common`  
```sh
sudo add-apt-repository \
   "deb [arch=amd64] https://mirrors.ustc.edu.cn/docker-ce/linux/debian \
   $(lsb_release -cs) \
   stable"
```

## date
* date "+%Y-%m-%d %H:%M:%S"
* 时间同步  
`ntpdate -u ntp.api.bz`
* 查看时区  
`date "+%Z"`
## tar 加密
```sh
tar -zcvf - $date.sql|openssl des3 -salt -k password -out $tarBackup/$date.tar.gz
或者
tar -zcvf - pma|openssl des3 -salt -k password | dd of=pma.des3
解压
dd if=pma.des3 |openssl des3 -d -k password|tar zxf -
```