## 切换2D模式
* `Super + Shift + Tab`
* `sudo deepin-feedback-cli` 日志收集
### 设置root 密码
`sudo password root //设置root密码`
## 终端环境之`oh-my-zsh`
### 1. install `zsh`

* 源码安装（貌似官方推荐安装最新的zsh）
* download: [官方源码zsh-5.5.1.tar.gz](http://zsh.sourceforge.net/Arc/source.html)   
* install:

```bash
//1. 解压
tar xzvf zsh-5.5.1.tar.gz
cd zsh-5.5.1/
//2. 编译安装
sudo ./configure // 提示缺少以下依赖库，先安装
sudo `apt-get install libncurses5-dev`
make
sudo make install //不知道为什么网上有的教程不加sudo？？
(3. 这个版本无此情况：可以查看zsh版本，但是我编译安装后，安装到/usr/local/bin/zsh 不知道为什么，所以一系列的坑由此开始。。。
zsh --version //提示zsh 不在/usr/bin/zsh 位置，当然不在
cd /usr/local/bin/ //到这个目录后 执行 `./zsh` 成功进入zsh，)但首次执行会提示`.~/.zshrc` 不存在，所以你看到的界面貌似是zsh添加这个配置文件的程序，可以直接退出，因为后面的oh-my-zsh 会替换这个配置文件 
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

### tmux 
* `sudo apt-get install tmux`
* 使用 见`./tmux.md`

### git
* sudo apt-get install git
* git clone git@github.com:pmzgit/note.git
### [官网](https://www.syntevo.com/smartgit/download/)
* [deepin 仓库](https://wiki.deepin.org/index.php?title=SmartGit) 
* `sudo apt-get install smartgit`
* 安装后jdk 指向jdk9 ，但是仓库版本（17.1）只能用jdk8 所以用sudo update-alternatives --config java 重新指向jdk8
* [破解](https://www.jianshu.com/p/79ff2d63ddc6)
* github auth `b0dadc04b6fc856a6a75` linux `42e22f76d67d2f145ee7` win7
* [环境变量http_proxy/https_proxy代理问题](https://blog.csdn.net/a1059682127/article/details/78632900)
### vsc
* [官网](https://code.visualstudio.com/) 
* `dpkg -i vsc.deb`
* 快捷键 见 `../tool/tools.md`

### [note](https://github.com/pmzgit/note.git)
* 自己的笔记库
### [dotfiles](https://github.com/pmzgit/dotfiles.git)
* 自己的配置库
### 有道云笔记
* 还是老实用官方网页版吧 

### shadowsocks
* [github](https://github.com/loliMay/shadowsocks-client)
* sudo dpkg -i shadowsocks-client_1.2.1_amd64.deb ; sudo aptitude update &&  sudo aptitude full-upgrade

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
* vim 配置见 `../tool/vim.md`
* 添加行号
```sh
" 注释
set nu
```
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

### dbeaver **推荐datagrip**
* 商店
* 需要jdk8

## catfish
`sudo apt install catfish`

## 触摸板驱动
`synclient HorizTwoFingerScroll=1`
### navicat
* https://www.navicat.com/download/navicat-for-mysql
* sudo /opt/navicat112_mysql_en_x64/start_navicat

### wget
* http://man.linuxde.net/wget


### nvm
* https://github.com/creationix/nvm
* 安装 `curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash`
* 使用 [用 apt-get 安装 node 和用 nvm 安装 node 的区别](https://www.jianshu.com/p/aab54ffdd060)
```sh
. ~/.nvm/nvm.sh
nvm ls-remote
nvm install 8.11.3
nvm ls
nvm use 
```
* => Close and reopen your terminal to start using nvm or run the following to use it now:

### yarn
* [安装](https://yarnpkg.com/zh-Hans/docs/install#debian-stable)
```sh
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -

echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list

sudo apt-get update

// 如果你使用nvm，可通过以下操作来避免node的安装

sudo apt-get install --no-install-recommends yarn

yarn --version

```
* 见 node.js.md

### [cerebro](https://github.com/KELiON/cerebro) **不推荐**

* 先安装nvm ，node，yarn
* 安装g++  
sudo apt-get install g++
* 安装
```sh
git clone https://github.com/KELiON/cerebro.git cerebro

cd cerebro && yarn && cd ./app && yarn && cd ../

// Run
yarn run dev
```
### [deepin-topbar](https://github.com/kirigayakazushin/deepin-topbar/wiki)


### nginx
* `apt-get install nginx`
* 配置文件地址 /etc/nginx/nginx.conf
注释掉默认配置
* 测试nginx配置文件
sudo nginx -t -c /etc/nginx/nginx.conf
* 测试通过，重启nginx服务器
/etc/init.d/nginx restart
### [ngrok](http://ngrok.ciqiuwl.cn/)
* 下载并解压，赋予执行权限
* `./ngrok -config=ngrok.cfg -subdomain 自己的自定义域名 80`
### lsof
* `sudo apt-get install lsof`


### aria2
* 安装 `apt-get install aria2`
* 用命令 `dpkg -L aria2` 可以查看安装到哪个目录了
* 安装好后，添加配置文件 aria2.conf
  [aria2 配置详解](http://aria2c.com/usage.html)
  [aria2.conf 示例文件](http://aria2c.com/archiver/aria2.conf)

### albert(亲测效果不行)
* http://www.ruanyifeng.com/blog/2013/07/gpg.html
* https://albertlauncher.github.io/docs/installing/
* alt+x
### teamviewer
* 商店
### 企业邮箱
* 网页版吧
### 企业微信，Tim
* 商店

### [postman](https://www.getpostman.com/apps)
* 解压直接执行根目录下的 Postman
* 或者  
`alias postman='sh -c "/home/pmz/soft/Postman/Postman"'`
### Redis Desktop Manager
* 商店
### Robo 3T
* 商店

### SecureCRT
* 官网下载，dpkg -i 安装
* 依赖安装`sudo apt install libjpeg8-dev`
* /usr/bin/SecureCRT 启动
* `sudo perl securecrt_linux_crack.pl /usr/bin/SecureCRT`
* 破解
```sh
	Name:		xiaobo_l
	Company:	www.boll.me
	Serial Number:	03-94-294583
	License Key:	ABJ11G 85V1F9 NENFBK RBWB5W ABH23Q 8XBZAC 324TJJ KXRE5D
	Issue Date:	04-20-2017
// 手动填写lisence
```
* 不能用vbs 脚本？那我安装它干嘛。。。。。

### [微信web开发](https://github.com/cytle/wechat_web_devtools) 
* [安装wine](https://wiki.deepin.org/index.php?title=Wine)
* [参考教程](https://www.cnblogs.com/cisum/p/7810346.html)
* [nwjs 下载地址](https://dl.nwjs.io/)

## 看图
* viewnior 商店

## [科学上网](https://noparkinghere.top/2017/01/25/2017/2017-01-25-ss-linux%E5%85%A8%E5%B1%80%E4%BB%A3%E7%90%86/)
# chrome 扩展
## [沙拉查词-网页划词翻译](https://github.com/crimx/crx-saladict/wiki)

## 谷歌访问助手 chrome 

## [Proxy SwitchyOmega](https://github.com/FelisCatus/SwitchyOmega/releases)
## 微博图床 

# 其他
* https://laod.cn/hosts/2018-google-hosts.html
*  `sudo systemctl restart NetworkManager`