# 前面
*  [参考1](https://zorro.gitbooks.io/poor-zorro-s-linux-book/content/shellbian-cheng-zhi-yu-fa-ji-chu.html)
* [shell 脚本库](https://gitee.com/aqztcom/kjyw/blob/master/shell/shell_manual.sh)

# shell 脚本学习
## 命令在Linux中的执行分为4个步骤
* 个人猜测首先进行：当参数中有通配符时，shell 首先进行参数的通配符路径扩展，然后再组装新的命令
* 第1步：判断用户是否以绝对路径或相对路径的方式输入命令（如/bin/ls），如果是的话则直接执行。
* 第2步：Linux系统检查用户输入的命令是否为“别名命令”，即用一个自定义的命令名称来替换原本的命令名称。可以用alias命令来创建一个属于自己的命令别名，格式为“alias 别名=命令”。若要取消一个命令别名，则是用unalias命令，格式为“unalias 别名”。
* 第3步：Bash解释器判断用户输入的是内部命令还是外部命令。内部命令是解释器内部的指令，会被直接执行；而用户在绝大部分时间输入的是外部命令，这些命令交由步骤4继续处理。可以使用“type 命令名称”来判断用户输入的命令是内部命令还是外部命令。
* 第4步：系统在多个路径中查找用户输入的命令文件，而定义这些路径的变量叫作PATH，可以简单地把它理解成是“解释器的小助手”，作用是告诉Bash解释器待执行的命令可能存放的位置，然后Bash解释器就会乖乖地在这些位置中逐个查找。PATH是由多个路径值组成的变量，每个路径值之间用冒号间隔，对这些路径的增加和删除操作将影响到Bash解释器对Linux命令的查找。
* env命令来查看到Linux系统中所有的环境变量,type cmd
* 顺序：  
别名：alias  
关键字：keyword  
函数：function  
内建命令：built in  
哈西索引：hash  
外部命令：command  
* alias功能在交互打开的bash中是默认开启的，但是在bash脚本中是默认关闭的  
shopt -s expand_aliases
* 内建命令
```sh
:,  ., [, alias, bg, bind, break, builtin, caller, cd, command, compgen, complete, compopt, continue, declare, dirs, disown, echo, enable, eval, exec, exit, export, false, fc,
   fg, getopts, hash, help, history, jobs, kill, let, local, logout, mapfile, popd, printf, pushd, pwd, read, readonly, return, set, shift, shopt, source, suspend,  test,  times,  trap,
   true, type, typeset, ulimit, umask, unalias, unset, wait
```
* 在交互式bash操作和bash编程中，hash功能总是打开的，我们可以用set +h关闭hash功能。
* bash在正常调用内部命令的时候并不会像外部命令一样产生一个子进程。
* 如果我们的bash程序没有显示的以一个exit指定返回码退出的话，那么其最后执行命令的返回码将成为整个bash脚本退出的返回码。    
    系统将错误返回码设计成1-255，这其中还分成两类：  
    1. 程序退出的返回码：1-127。这部分返回码一般用来作为给程序员自行设定错误退出用的返回码，比如：如果一个文件不存在，ls将返回2。如果要执行的命令不存在，则bash统一返回127。返回码125盒126有特殊用处，一个是程序命令不存在的返回码，另一个是命令的文件在，但是不可执行的返回码。
    2. 程序被信号打断的返回码：128-255。这部分系统习惯上是用来表示进程被信号打断的退出返回码的。一个进程如果被信号打断了，其退出返回码一般是128+信号编号的数字。如果一个进程被2号信号打断的话，其返回码一般是128+2=130
* fd  
在内核中，文件描述符的数组所直接映射的实际上是文件表，文件表再索引到相关文件的v_node。具体可以参见《UNIX系统高级编程》。  
shell在产生一个新进程后，新进程的前三个文件描述符都默认指向三个相关文件。这三个文件描述符对应的数组下标分别为0，1，2。0对应的文件叫做标准输入（stdin），1对应的文件叫做标准输出（stdout），2对应的文件叫做标准报错(stderr)。但是实际上，默认跟人交互的输入是键盘、鼠标，输出是显示器屏幕，这些硬件设备对于程序来说都是不认识的，所以操作系统借用了原来“终端”的概念，将键盘鼠标显示器都表现成一个终端文件。于是stdin、stdout和stderr就最重都指向了这所谓的终端文件上。  
进程可以打开多个文件，多个描述符之间都可以进行重定向。当然，输入也可以，比如：3<表示从描述符3读取。
* 重定向
```sh
Here strings：
cat <<< asdasdasd

Here Document：
cat << EOF

```
## 管道
* Linux上的管道就是一个操作方式为文件的内存缓冲区。
* Linux上的管道分两种类型：  
匿名管道  |（只包含标准输出） 或者 |& （包含标准输出和标准报错）
命名管道  
这两种管道也叫做有名或无名管道。匿名管道最常见的形态就是我们在shell操作中最常用的"|"。它的特点是只能在父子进程中使用，父进程在产生子进程前必须打开一个管道文件，然后fork产生子进程，这样子进程通过拷贝父进程的进程地址空间获得同一个管道文件的描述符，以达到使用同一个管道通信的目的。此时除了父子进程外，没人知道这个管道文件的描述符，所以通过这个管道中的信息无法传递给其他进程。这保证了传输数据的安全性，当然也降低了管道了通用性，于是系统还提供了命名管道。

我们可以使用mkfifo或mknod命令来创建一个命名管道，这跟创建一个文件没有什么区别：

### () {}
(list)：放在()中执行的命令将在一个subshell环境中执行，这样的命令将打开一个bash子进程执行。即使要执行的是内建命令，也要打开一个subshell的子进程。另外要注意的是，当内建命令前后有管道符号连接的时候，内建命令本身也是要放在subshell中执行的。这个subshell子进程的执行环境基本上是父进程的复制，除了重置了信号的相关设置。bash编程的信号设置使用内建命令trap，将在后续文章中详细说明。+

{ list; }：大括号作为函数语法结构中的标记字段和list标记字段，是一个关键字。在大括号中要执行的命令列表（list）会放在当前执行环境中执行。命令列表必须以一个换行或者分号作为标记结束。

## [前台任务，后台任务，守护进程](http://www.ruanyifeng.com/blog/2016/02/linux-daemon.html)
* "后台任务"与"前台任务"的本质区别只有一个：是否继承标准输入。所以，执行后台任务的同时，用户还可以输入其他命令。
* & 或　ｃｔｒｌ + z:表明这个命令的执行放到jobs中跑，bash不必wait进程
* nohup命令对server.js进程做了三件事。

阻止SIGHUP信号发到这个进程。  
关闭标准输入。该进程不再能够接收任何输入，即使运行在前台。  
重定向标准输出和标准错误到文件nohup.out。  
## 脚本调试
-v 参数就是可视模式,它会在执行bash程序的时候将要执行的内容也打印出来，除此之外，并不改变bash执行的过程  
-x参数是跟踪模式(xtrace)。可以跟踪各种语法的调用，并打印出每个命令的输出结果  
-n参数用来检查bash的语法错误，并且不会真正执行bash脚本  
-e参数，这个参数可以让bash脚本命令执行错误的时候直接退出，而不是继续执行。


## 用户 用户组
useradd test

passwd test  
增加用户test，有一点要注意的，useradd增加一个用户后，不要忘了给他设置密码，不然不能登录的。  

usermod -d /home/test -G test2 test  

将test用户的登录目录改成/home/test，并加入test2组，注意这里是大G。  
userdel test  
cat /etc/passwd

gpasswd -a test test2 将用户test加入到test2组  
gpasswd -d test test2 将用户test从test2组中移出  

groupadd  test  
groupmod -n test2  test  
将test组的名子改成test2  
groupdel test2  
查看当前登录用户所在的组 groups，查看apacheuser所在组groups apacheuser   
所有组 cat /etc/group  

查看当前登录用户 w whoami  
id apacheuser  
last 查看登录成功的用户记录  
lastb 查看登录不成功的用户记录  
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

## 设置别名 /etc/bashrc
```shell
alias cd1="cd ../"
alias cd2="cd ../../"
alias cd4="cd ../../../../"
alias cd3="cd ../../../"
alias grep="grep --color"
alias egrep="egrep --color"
```
## [shell 基本语法](https://www.linuxprobe.com/chapter-04.html#423)

* if后面是一个命令(严格来说是list)  
根据bash的定义，list就是若干个使用管道，；，&，&&，||这些符号串联起来的shell命令序列，结尾可以；，&或换行结束。这个定义可能比较复杂，如果暂时不能理解，大家直接可以认为，if后面跟的就是个shell命令。换个角度说，bash编程仍然贯彻了C程序的设计哲学，即：一切皆表达式。
* 一切皆表达式这个设计原则，确定了shell在执行任何东西（注意是任何东西，不仅是命令）的时候都会有一个返回值，因为根据表达式的定义，任何表达式都必须有一个值。在bash编程中，这个返回值也限定了取值范围：0-255。跟C语言含义相反，bash中0为真（true），非0为假（false）。这就意味着，任何给bash之行的东西，都会反回一个值
* case分支和for循环,跟if 和 while，until 不同，它们所判断的不再是“表达式”是否为真，而是去匹配字符串。
* 在[] 表达式中，常见的>,<需要加转义字符，表示字符串大小比较，以acill码 位置作为比较。 不直接支持<>运算符，还有逻辑运算符|| && 它需要用-a[and] –o[or]表示
* [[]] 运算符只是[]运算符的扩充。能够支持<,>符号运算不需要转义符，它还可以以字符串比较大小。里面支持逻辑运算符：|| &&；可以支持的判断参数 用help test
* 逻辑运算符
1.	关于档案与目录的侦测逻辑卷标！  
-f	常用！侦测『档案』是否存在 eg: if [ -f filename ]  
-d	常用！侦测『目录』是否存在  
-b	侦测是否为一个『 block 档案』  
-c	侦测是否为一个『 character 档案』  
-S	侦测是否为一个『 socket 标签档案』  
-L	侦测是否为一个『 symbolic link 的档案』  
-e	侦测『某个东西』是否存在！  
2.	关于程序的逻辑卷标！
-G	侦测是否由 GID 所执行的程序所拥有
-O	侦测是否由 UID 所执行的程序所拥有
-p	侦测是否为程序间传送信息的 name pipe 或是 FIFO （老实说，这个不太懂！）
3.	关于档案的属性侦测！
-r	侦测是否为可读的属性
-w	侦测是否为可以写入的属性
-x	侦测是否为可执行的属性
-s	侦测是否为『非空白档案』
-u	侦测是否具有『 SUID 』的属性
-g	侦测是否具有『 SGID 』的属性
-k	侦测是否具有『 sticky bit 』的属性
4.	两个档案之间的判断与比较 ；例如[ test file1 -nt file2 ]
-nt	第一个档案比第二个档案新
-ot	第一个档案比第二个档案旧
-ef	第一个档案与第二个档案为同一个档案（ link 之类的档案）
5.	逻辑 
&&	逻辑的 AND 的意思  
||	逻辑的 OR 的意思  
叹号 ! 代表对命令(表达式)的`返回值`取反 

* 运算符
=	等于 应用于：整型或字符串比较 如果在[] 中，只能是字符串  
!=	不等于 应用于：整型或字符串比较 如果在[] 中，只能是字符串  
<	小于 应用于：整型比较 在[] 中，不能使用 表示字符串  
`>`大于 应用于：整型比较 在[] 中，不能使用 表示字符串  
-eq	等于 应用于：整型比较  
-ne	不等于 应用于：整型比较  
-lt	小于 应用于：整型比较  
-gt	大于 应用于：整型比较  
-le	小于或等于 应用于：整型比较  
-ge	大于或等于 应用于：整型比较  
-a	双方都成立（and） 逻辑表达式 –a 逻辑表达式  
-o	单方成立（or） 逻辑表达式 –o 逻辑表达式  
-z	空字符串  
-n	非空字符串  

* 脚本参数
$0：命令名。  
$n：n是一个数字，表示第n个参数。  
$#：参数个数。  
$*：所有参数列表。  
$@：同上。  
$?：上一个命令的返回值。  
$$：当前shell的PID。  
$!：最近一个被放到后台任务管理的进程PID。  
$-：列出当前bash的运行参数，比如set -v或者-i这样的参数。  
$_："_"算是所有特殊变量中最诡异的一个了，在bash脚本刚开始的时候，它可以取到脚本的完整文件名。当执行完某个命令之后，它可以取到，这个命令的最后一个参数。当在检查邮件的时候，这个变量帮你保存当前正在查看的邮件名。  
上述所有取值都可以写成${}的方式，因为bash中对变量取值有两种写法，另外一种是${aaa}。  
内建命令shift可以用来对参数进行位置处理，它会将所有参数都左移一个位置，可以用来进行参数处理。
### 数组
```sh
#!/bin/bash
#定义一个一般数组
declare -a array

#为数组元素赋值
array[0]=1000
array[1]=2000
array[2]=3000
array[3]=4000

#直接使用数组名得出第一个元素的值
echo $array
#取数组所有元素的值
echo ${array[*]}
echo ${array[@]}
#取第n个元素的值
echo ${array[0]}
echo ${array[1]}
echo ${array[2]}
echo ${array[3]}
#数组元素个数
echo ${#array[*]}
#取数组所有索引列表
echo ${!array[*]}
echo ${!array[@]}

#定义一个关联数组
declare -A assoc_arr

#为关联数组复制
assoc_arr[zorro]='zorro'
assoc_arr[jerry]='jerry'
assoc_arr[tom]='tom'

#所有操作同上
echo $assoc_arr
echo ${assoc_arr[*]}
echo ${assoc_arr[@]}
echo ${assoc_arr[zorro]}
echo ${assoc_arr[jerry]}
echo ${assoc_arr[tom]}
echo ${#assoc_arr[*]}
echo ${!assoc_arr[*]}
echo ${!assoc_arr[@]}

```

### 特殊符号

#### {}
* 用类似枚举的方式创建一些目录  
```sh
mkdir -p test/zorro/{a,b,c,d}{1,2,3,4}  
ls test/zorro/  
a1  a2  a3  a4  b1  b2  b3  b4  c1  c2  c3  c4  d1  d2  d3  d4  
```
* mv test/{a,c}.conf  
意思是 mv test/a.conf test/c.conf

#### ～
在bash中一般表示用户的主目录。cd ~表示回到主目录。cd ~zorro表示回到zorro用户的主目录。

#### 变量：我们都知道取一个变量值可以用$或者${}。在使用${}的时候可以添加很多对变量进行扩展操作的功能
```sh
variable 变量不被更改:
${variable:-val}
如果变量variable是空值或者没有赋值，则此表达式取值为val，variable变量不被更改，以后还是空。如果variable已经被赋值，则此表达式取值为variable 变量值
${variable:+val}
与上面相反
---
variable 变量被更改：
${variable:=val}
variable未被赋值，则赋值成＝后面的值
---
${aaa:?unset}
判断变量是否未定义或为空，如果符合条件，就提示？后面的字符串。并输出标准错误，命令返回值为1；如果已被赋值，则表达式值为变量值
---
取字符串偏移量，表示取出aaa变量对应字符串的第10个字符之后的字符串(起始索引为0，闭区间)，第二个数字表示取多长，变量原值不变。
${aaa:start:count}
---
路径匹配删除：
匹配左删除
${path-var##path-parrtern} ${path-var#path-parrtern}
#：从前到后，匹配第一个
##：从后到前，匹配最后一个
变量paramenter看做字符串从左往右找到第一个匹配字串，取其后面的字串
匹配右删除
%：从前到后，匹配第一个
%%：从后到前，匹配最后一个
${path-var%path-parrtern} ${path-var%%path-parrtern}

---
字符串替换，将pattern匹配到的第一个字符串替换成string，pattern可以使用通配符
${var/pattern/string}
全局替换
${var//pattern/string}
---
${!B*}
${!B@}
取出所有以B开头的变量名,请注意他们跟数组中相关符号的差别
---
大写转换
${str-var^} ${str-var^^} 首字母 全局
小写转换
${str-var,} ${str-var,,} 首字母 全局
---
${#aaa}
取变量长度
```
### bash会执行放在``号中的命令。在某些情况下双反引号的表达能力有欠缺，比如嵌套的时候就分不清到底是谁嵌套谁？所以bash还提供另一种写法，跟这个符号一样就是$()。

## [expr](http://man.linuxde.net/expr)
```sh
expr的常用运算符：

加法运算：+
减法运算：-
乘法运算：\*
除法运算：/
求摸（取余）运算：%

result=`expr 2 \* 3`
result=$(expr $no1 + 5)
$[]或者 $(())可以得到一个算数运算的值
echo $[213*456]
这个运算只支持整数，并且对与小数只娶其整数部分（没有四舍五入，小数全舍）。这个计算方法是bash提供的基础计算方法，如果想要实现更高级的计算可以使用let命令。如果想要实现浮点数运算，我一般使用awk来处理。
i=2
let ++i
let i=i**2
let的另外一种写法是(())
((i+=4))
((i=i**7))
```

### 进程置换
<(list) 和 >(list)
这两个符号可以将list的执行结果当成别的命令需要输入或者输出的文件进行操作，这个符号会将相关命令的输出放到/dev/fd中创建的一个管道文件中，并将管道文件作为参数传递给相关命令进行处理。

### 路径匹配扩展 shopt -s extglob

?(pattern-list)：匹配所给pattern的0次或1次；   
*(pattern-list)：匹配所给pattern的0次以上包括0次；  
+(pattern-list)：匹配所给pattern的1次以上包括1次；   
@(pattern-list)：匹配所给pattern的1次；   
!(pattern-list)：匹配非括号内的所给pattern。  



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

## [shopt](http://man.linuxde.net/shopt)

## [curl命令的基本使用](https://www.cnblogs.com/yinzhengjie/p/7719804.html)
## sed
Stream Editor文本流编辑，sed是一个“非交互式的”面向字符流的编辑器。能同时处理多个文件多行的内容，可以不对原文件改动，把整个文件输入到屏幕,可以把只匹配到模式的内容输入到屏幕上。还可以对原文件改动，但是不会再屏幕上返回结果。
### 保持和获取：h命令和G命令  
在sed处理文件的时候，每一行都被保存在一个叫模式空间的临时缓冲区中，除非行被删除或者输出被取消，否则所有被处理的行都将 打印在屏幕上。接着模式空间被清空，并存入新的一行等待处理。

`sed -e '/test/h' -e '$G' file`  
在这个例子里，匹配test的行被找到后，将存入模式空间，h命令将其复制并存入一个称为保持缓存区的特殊缓冲区内。第二条语句的意思是，当到达最后一行后，G命令取出保持缓冲区的行，然后把它放回模式空间中，且追加到现在已经存在于模式空间中的行的末尾。在这个例子中就是追加到最后一行。简单来说，任何包含test的行都被复制并追加到该文件的末尾。


### sed命令的语法格式：

sed的命令格式： sed [option] 'sed command'filename

sed的脚本格式：sed [option] -f 'sed script'filename

### sed命令的选项(option)：

-n ：只打印模式匹配的行

-e ：直接在命令行模式上进行sed动作编辑，此为默认选项，多个选项，每个操作用 -e 参数,按顺序执行

-f ：将sed的动作写在一个文件内，用–f filename 执行filename内的sed动作

-r ：支持扩展表达式

-i ：直接修改文件内容

### 定界符
/ 或其他
## sed在文件中查询文本的方式：

#### 1. 使用行号，可以是一个简单数字，或是一个行号范围

x x为行号，从1开始,$ 表示最后一行，

x,y 表示行号从x到y

* 选定行的范围：,（逗号）

sed '/test/,/west/s/$/aaa bbb/' file 对于模板test和west之间的行，每行的末尾用字符串aaa bbb替换：

/pattern/ 查询包含模式的行

/pattern/pattern/ **按顺序**，查找匹配模式的行

/pattern/,x **按顺序**在给定行号上查询包含模式的行

x,/pattern/  **按顺序**通过行号和模式查询匹配的行

x,y! **取反**查询不包含指定行号x和y的行

* 已匹配字符串标记&  
echo this is a test line | sed 's/\w\+/[&]/g'

* 多表达式  分号 **；** ，逻辑和   
sed '表达式; 表达式'
例子：  
`sed -n '/wf/,/jj/p' ori.txt`

#### 2. 使用正则表达式、扩展正则表达式(必须结合-r选项)

## sed的编辑命令(sed command)
#### 对文件的操作无非就是”增删改查“
* p 打印
* d 删除  
sed '/^$/d' file 删除空白行
* s 替换  
nl err.txt |sed 's/s/A/'   
sed 's/sk/SK/3g' /ng 全行内匹配（n 列索引）

* 从文件读入：r命令  
file里的内容被读进来，显示在与test匹配的行后面，如果匹配多行，则file的内容将显示在所有匹配行的下面：    

sed '/test/r file' filename  
* 写入文件：w命令    
在example中所有包含test的行都被写入file里：

sed -n '/test/w file' example  
* 追加（行下）：a\命令  
将 this is a test line 追加到 以test 开头的行后面：  

sed '/^test/a\this is a test line' file   
在 test.conf 文件第2行之后插入 this is a test line：

sed -i '2a\this is a test line' test.conf  
* 插入（行上）：i\命令   
将 this is a test line 追加到以test开头的行前面：  

sed '/^test/i\this is a test line' file  
在test.conf文件第5行之前插入this is a test line：  

sed -i '5i\this is a test line' test.conf  
* 下一个：n命令  
如果test被匹配，则移动到匹配行的下一行，替换这一行的aa，变为bb，并打印该行，然后继续：  
 
sed '/test/{ n; s/aa/bb/; }' file  



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

## tee
把stdin 重定向到给定文件和stdout上。  
存在缓存机制，每1024个字节将输出一次。若从管道接收输入数据，应该是缓冲区满，才将数据转存到指定的文件中。若文件内容不到1024个字节，则接收完从标准输入设备读入的数据后，将刷新一次缓冲区，并转存数据到指定文件。

* -a：向文件中重定向时使用追加模式；

## [dig](http://roclinux.cn/?p=2449)

## 查看外网 ip
* curl ip.cn
* `curl cip.cc`
* `curl ifconfig.me`  

## sh, source, exec 执行脚本区别
* 使用 sh script.sh执行脚本时，当前shell是父进程，生成一个子shell进程，在子shell中执行脚本。脚本执行完毕，退出子shell，回到当前shell。  
`./script.sh 与 sh script.sh等效`
* source命令也称为“点命令”，也就是一个点符号（.）。在当前上下文中执行脚本，不会生成新的进程。脚本执行完毕，回到当前shell。因此当前上下文的变量对该脚本是可见的。 source命令通常用于重新执行刚修改的初始化文件，使之立即生效，而不必注销并重新登录。 
`source filename 或 . filename`

* 使用exec command方式，会用command进程替换当前shell进程，并且保持PID不变。执行完毕，直接退出，不回到之前的shell环境。

* 在sh和source方式下，脚本执行完毕，都会回到之前的shell中。但是两种方式对上下文的影响不同

## shell内置变量 都需要加s双引号
`$*` is a single string, whereas `$@` is an actual array

## 目录下的所有文件中查找字符串

find .| xargs grep -rn patern 

## 清空文件
`:> /tmp/out`  
":"是一个内建命令，跟true是同样的功能，所以没有任何输出，所以这个命令清空文件的作用。