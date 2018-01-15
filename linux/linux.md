# Bash Shell 中文件名不同颜色的含义
* 蓝色－目录
* 绿色－可执行文件
* 红色－压缩文件
* 浅蓝色－链接文件
* 灰色－其他文件
* 紫色－图形文件
* 黄色－设备文件
* 棕色－FIFO文件（先进先出，命令管道）

# 查看系统信息
* 发行版本：cat /etc/issue  或者 cat /etc/debian_version 
* 内核版本：cat /proc/version
* 设置http代理：  export http_proxy=http://hostname:port
* 修改当前用户密码：  
修改用户密码步骤：  
1、命令行下输入命令passwd 用户名  
2、提示你输入当前用户的当前密码  
3、输入新密码  
4、再次输入新密码  
5、输入命令exit退出  
6、用新密码登录

# 终端快捷键
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

# linux shell通配符(wildcard) [转载](http://www.cnblogs.com/chengmo/archive/2010/10/17/1853344.html)
通配符是由shell处理的（不是由所涉及到命令语句处理的，其实我们在shell各个命令中也没有发现有这些通配符介绍）, 它只会出现在 命令的“参数”里（它不用在 命令名称里， 也不用在 操作符上）。当shell在“参数”中遇到了通配符时，shell会将其当作路径或文件名去在磁盘上搜寻可能的匹配：若符合要求的匹配存在，则进行代换(路径扩展)；否则就将该通配符作为一个普通字符传递给“命令”，然后再由命令进行处理。总之，通配符 实际上就是一种shell实现的路径扩展功能。在 通配符被处理后, shell会先完成该命令的重组，然后再继续处理重组后的命令，直至执行该命令。

## 字符	含义
* `*`	匹配 0 或多个字符 
* **表示任意一层子目录 
* ?	匹配任意一个字符  
* [list]	匹配 list 中的任意单一字符  
* [!list]	匹配 除list 中的任意单一字符以外的字符  
* [c1-c2]	匹配 c1-c2 中的任意单一字符 如：[0-9] [a-z]  
* {string1,string2,...}	匹配 sring1 或 string2 (或更多)其一字符串
* {c2..c2}	匹配 c1-c2 中全部字符 如{1..10}  
#### 需要说明的是：通配符看起来有点象正则表达式语句，但是它与正则表达式不同的，不能相互混淆。把通配符理解为shell 特殊代号字符就可。而且涉及的只有，*,? [] ,{} 这几种。

## shell元字符（特殊字符 Meta）
* IFS	由 `<space>` 或 `<tab>` 或 `<enter>` 三者之一组成(我们常用 space )。  
* CR	由 `<enter>` 产生。  
* =	设定变量。  
* $	作变量或运算替换(请不要与 shell prompt 搞混了)。  
* `>`	重导向 stdout。 *  
* <	重导向 stdin。 *  
* |	命令管线。 *  
* &	重导向 file descriptor ，或将命令置于背境执行。或并行执行执行命令。例如：`npm run script1.js & npm run script2.js`  
* ( )	将其内的命令置于 nested subshell 执行，或用于运算或命令替换。 *  
* { }	将其内的命令置于 non-named function 中执行，或用在变量替换的界定范围。  
* ;	在前一个命令结束时，而忽略其返回值，继续执行下一个命令。 *  
* &&	在前一个命令结束时，若返回值为 true，继续执行下一个命令。 *  
* ||	在前一个命令结束时，若返回值为 false，继续执行下一个命令。 *  
* !	执行 history 列表中的命令。*  
#### 加入”*” 都是作用在命令名直接。可以看到shell 元字符，基本是作用在命令上面，用作多命令分割（或者参数分割）。因此看到与通配符有相同的字符，但是实际上作用范围不同。所以不会出现混淆。

#### 以下是man bash 得到的英文解析：
```shell
metacharacter  
              A character that, when unquoted, separates words.  One of the following:  
              |  & ; ( ) < > space tab  
control operator   
              A token that performs a control function.  It is one of the following symbols:
              || & && ; ;; ( ) | <newline>
```

# nohup 和 & 后台执行 [转载](http://blog.csdn.net/zhang_red/article/details/52789691)
* nohup是永久执行
* & 是指在后台运行
* 就是指，用nohup运行命令可以使命令永久的执行下去，和用户终端没有关系，例如我们断开SSH连接都不会影响他的运行，注意了nohup没有后台运行的意思；&才是后台运行
* 那么，我们可以巧妙的吧他们结合起来用就是 `nohup COMMAND &`这样就能使命令永久的在后台执行
* nohup执行后，会产生日志文件，把命令的执行中的消息保存到这个文件中，一般在当前目录下，如果当前目录不可写，那么自动保存到执行这个命令的用户的home目录下，例如root的话就保存在/root/下
# man
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
# 作业管理
1. 将“当前”作业放到后台“暂停”：ctrl+z
 
2. 观察当前后台作业状态：jobs
参数：
 -l 除了列出作业号之外同时列出PID  
 -r：列出仅在后台运行（run）的作业
 -s：仅列出暂停的作业
 
3. 将后台作业拿到前台处理：fg   
 fg %jobnumber (%可有可无)
 
4. 让作业在后台运行：bg
ctrl+z让当前作业到后台去暂停，bg 作业号就可以在后台run
 
5. 管理后台作业：kill  
我们可以让一个已经在后台的作业继续执行，也可以让该作业使用fg拿到前台。如果直接删除该作业，或者让作业重启，需要给作业发送信号。
kill -signal %jobnumber
参数：
-l 列出当前kill能够使用的信号
signal：表示给后台的作业什么指示，用man 7 signal可知
-1（数字）：重新读取一次参数的设置文件（类似reload）
-2：表示与由键盘输入ctrl-c同样的动作
-9：立刻强制删除一个作业
-15：以正常方式终止一项作业。与-9不一样。

# find
### 1. 注意 find 命令的路径是作为第一个参数的， 基本命令格式为  
find [path] [option] [表达式] [action]
### 2. 使用
* 指定目录下搜索指定文件名的文件：  
$ find /etc/ -name "通配符表达式"
* 通过正则匹配   
`find . -regex ".*\(\.txt\|\.pdf\)$"`   
`-iregex 忽略大小写`
* 限定路径层级   
`find . -maxdepth 1 -iname '*mongo*'`

* 通过文件类型   
`find . -type f` 文件类型   
`find . -type d` 文件夹  

* 与时间相关的命令参数：

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

### 参考
* [find 文档](http://man.linuxde.net/find "find")
* [Linux 命令行：find 的 26 个用法示例](http://www.codebelief.com/article/2017/02/26-examples-of-find-command-on-linux/)

# grep

# 文件压缩
## zip unzip 
* 安装 
`yum install -y unzip zip`
* zip   
-r 递归打包 -o 输出文件，其后紧跟打包输出文件名 -q安静模式 -【1-9】压缩级别，默认为最高9  -x 排除文件不压缩，只能使用绝对路径，否则不起作用。  
zip -r -9 -q -o shiyanlou_9.zip /home/shiyanlou -x ~/*.zip
* -e 加密 -l参数将LF转换为CR+LF 兼容windows  
* du命令查看打包后文件的大小  du -h -d 0 *.zip ~ | sort  
-d, --max-depth（所查看文件的深度）
* unzip -d 目标目录 -l 不解压只查看压缩包内容 -O（大写） 指定编码 
```sh 
unzip -q shiyanlou.zip -d ziptest  
unzip -l shiyanlou.zip  
unzip -O GBK 中文压缩文件.zip 
``` 
## rar
rar a 添加一个目录  d删除某个文件 l 查看不解压  
rar a shiyanlou.rar .  
rar d shiyanlou.rar .zshrc  
rar l shiyanlou.rar  
unrar x 全路经解压  e 去掉路径解压  
unrar x shiyanlou.rar  
mkdir tmp  
$ unrar e shiyanlou.rar tmp/  

## tar 
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
## uniq
> 管道过滤唯一字符  

## ln [创建链接：官方文档](http://man.linuxde.net/ln)
## 查看当前服务和监听端口  `sudo netstat -atunlp | grep mongo`
## ubuntu 修改主机名
```
//查看主机名
topcom@master:~$ hostname
master
//永久修改
vim /etc/hostname
```
## 复制符号连接，保留文件属性，递归操作 -dpr
`cp -av source dest`

## 查看硬链接ln关联的所有文件及路径  
```bash
ls -i  myInfo.txt
3814056 myInfo.txt
find / -inum 3814056 
/home/homer/me/myInfo.txt
/home/homer/me/.me/myInfo.txt_ln
/home/homer/bin/myInfo.txt
```
## strings 命令
* 在对象文件或二进制文件查找可打印的字符串
`Print all strings in a binary: strings file`
`strings /usr/lib64/libssl.so |grep OpenSSL`
