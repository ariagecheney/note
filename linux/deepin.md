## 切换2D模式
* `Super + Shift + Tab`

## 终端环境之`oh-my-zsh`
### 1. install `zsh`

* 源码安装（貌似官方推荐安装最新的zsh）
* download: [官方源码zsh-5.2.tar.gz](http://zsh.sourceforge.net/Arc/source.html)   
* install:

```bash
//1. 解压
tar xzvf zsh-5.2.tar.gz
cd zsh-5.2/
//2. 编译安装
./configure // 提示缺少以下依赖库，先安装
`apt-get install libncurses5-dev`
make
sudo make install //不知道为什么网上有的教程不加sudo？？
//3. 可以查看zsh版本，但是我编译安装后，安装到/usr/local/bin/zsh 不知道为什么，所以一系列的坑由此开始。。。
zsh --version //提示zsh 不在/usr/bin/zsh 位置，当然不在
cd /usr/local/bin/ //到这个目录后 执行 `./zsh` 成功进入zsh，但首次执行会提示`.~/.zshrc` 不存在，所以你看到的界面貌似是zsh添加这个配置文件的程序，根据提示操作 
//4. make zsh your default shell 用`chsh`命令
//首先你可以查看ubuntu里面都安装了哪些shell
cat /etc/shells
chsh -s $(which zsh) //fail，因为zsh并没有添加到上面那个文件里
//所以zsh必须添加到`/etc/shells`里，才能把zsh改成登陆用户默认的shell
sudo vim /etc/shells // 添加`/usr/local/bin/zsh`
chsh -s $(which zsh) //输入密码 
// 重启系统，打开shell 你会看到已经进入到zsh
echo $SHELL // 查看当前正在运行的shell
=/usr/local/bin/zsh
```

* 参考：[install zsh](https://github.com/robbyrussell/oh-my-zsh/wiki/Installing-ZSH)
 
### 2. 使用`oh-my-zsh`的配置文件  
```bash
// 执行下面命令
sh -c "$(wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
```

* 参考:[github oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh)
* 需要提前安装git

### git
* sudo apt-get install git
* git clone git@github.com:pmzgit/note.git
### [官网](https://www.syntevo.com/smartgit/download/)
* [deepin 仓库](https://wiki.deepin.org/index.php?title=SmartGit) 
* `sudo apt-get install smartgit`
* 安装后jdk 指向jdk9 ，但是仓库版本（17.1）只能用jdk8 所以用sudo update-alternatives --config java 重新指向jdk8
* [破解](https://www.jianshu.com/p/79ff2d63ddc6)
* github auth `b0dadc04b6fc856a6a75` linux `42e22f76d67d2f145ee7` win7

### vsc
* [官网](https://code.visualstudio.com/) 
* `dpkg -i vsc.deb`
* 快捷键 见 `../tool/tools.md`

### 有道云笔记
* 还是老实用官方网页版吧 

### lantern
* [releases](https://github.com/getlantern/lantern/releases/tag/latest)
* dpkg -i deb
* `lantern 命令失败，需要安装下面的包`
* `apt-cache search libappindicator3`
* `apt-get install libappindicator3-1`

### atom
* 商店

### vim
* vim --version
* vim —version 里如果有 xterm_clipboard 支持系统剪切板寄存器
* sudo apt-get install vim-gui-common 能支持"+ 剪切板
* /usr/bin/vim .gvim
### teamviewer
* 商店

### jdk
* mkdir /usr/lib/jvm
* 下载jdk tar 文件
* sudo tar xzvf jdk-8u172-linux-x64.tar.gz -C /usr/lib/jvm/
* sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.8.0_172/bin/java 100
* sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.8.0_172/bin/javac 100
* java -version
* javac -version
* update-altenativess --display java
* update-altenativess --config java
* sudo vim /etc/profile
```Shell
#java
export JAVA_HOME=/usr/lib/jvm/jdk1.8.0_172
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=${JAVA_HOME}/lib:${JRE_HOME}/lib
#export IDEA_JDK_64=/usr/lib/jvm/jdk1.8
#export IDEA_JDK=/usr/lib/jvm/jdk1.8
export PATH=${JAVA_HOME}/bin:$PATH
# maven
export MAVEN_HOME=/home/pmz/code/apache-maven-3.5.3
export PATH=${PATH}:${MAVEN_HOME}/bin
# tomcat
export TOMCAT_HOME=/home/pmz/code/apache-tomcat-8.0.52
export CATALINA_HOME=/home/pmz/code/apache-tomcat-8.0.52
```
* . /etc/profile // 使配置生效

### svn 客户端和服务端
* `sudo apt-get install subversion`
* 后边idea 需要用svn 命令行

### idea
* 商店安装 JetBrains Toolbox
* 然后 安装idea datagrap 之类的
* 注意/etc/hosts

~~[本地服务器激活](http://blog.lanyus.com/archives/174.html)~~  
~~`/usr/local/bin/idea`~~  
~~sudo ./IntelliJIDEALicenseServer_linux_amd64~~ 
~~`./Downloads/idea-IU-163.12024.16/bin/idea.sh `~~

### tomcat
* 下载解压
* . /etc/profile // 配置环境变量，见上面，并运行此命令使配置生效
* idea 先配置tomcat
* /bin/Catalina.sh java_home
* tomcat-users.xml



### mysql
* sudo apt-get install mysql-server mysql-client
* /etc/mysql/my.conf 配置
* /var/lib/mysql 数据库文件 需要迁移
* /usr/bin/mysql mysql 的脚本都在这
* sudo service mysql status
* 密码 root
* sudo /usr/bin/mysqladmin -uroot -p shutdown
* sudo nohup /usr/bin/mysqld_safe &  启动

### dbeaver
* 商店
* 需要jdk8


### navicat
* https://www.navicat.com/download/navicat-for-mysql
* sudo /opt/navicat112_mysql_en_x64/start_navicat

### wget
* http://man.linuxde.net/wget


### nvm
* https://github.com/creationix/nvm
* `wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.1/install.sh | bash`
* => Appending nvm source string to /home/pmz/.zshrc
* => Close and reopen your terminal to start using nvm or run the following to use it now:

### nginx
* `apt-get install nginx`
* 配置文件地址 /etc/nginx/nginx.conf
* 测试nginx配置文件
nginx -t -c /etc/nginx/nginx.conf
* 测试通过，重启nginx服务器
/etc/init.d/nginx restart


### aria2
* 安装 `apt-get install aria2`
* 用命令 `dpkg -L aria2` 可以查看安装到哪个目录了
* 安装好后，添加配置文件 aria2.conf
  [aria2 配置详解](http://aria2c.com/usage.html)
  [aria2.conf 示例文件](http://aria2c.com/archiver/aria2.conf)

### albert
* http://man.linuxde.net/apt-key
* http://www.ruanyifeng.com/blog/2013/07/gpg.html
* https://albertlauncher.github.io/docs/installing/
* alt+x

### todo
* VMware虚拟机：C:\Users\eversec\Documents\Virtual Machines\Debian 8.x
* postman
* handshaker
* redisdesk
* robo3t
* secure3t
* tim
* vsc
* 热点
* 企业微信
* 邮箱大师
* 有道词典
* 微信web开发 https://github.com/cytle/wechat_web_devtools
* 有道云笔记