### zsh
* `apt-get install libncurses5-dev`
* 更新博客需要提前安装git

### git
* apt-get 源安装
* 看截图
* git clone git@github.com:pmzgit/note.git
* smartgit 商店
* github auth `b0dadc04b6fc856a6a75` linux `42e22f76d67d2f145ee7` win7


### lantern
* `lantern`
* `apt-cache search libappindicator3`
* `apt-get install libappindicator3-1`

### atom
* 商店

### vim
* vim --version
* vim —version | grep xterm_clipboard 是否支持系统剪切板寄存器
* sudo apt-get install vim-gui-common 能支持"+ 剪切板
* /usr/bin/vim .gvim
### teamviewer
* 商店

### jdk
* /usr/lib/jvm
* /usr/bin/java
* update-altenativess --display java
* update-altenativess --config java
* /etc/profile
```Shell
# java path
export JAVA_HOME=/usr/lib/jvm/jdk1.7
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=${JAVA_HOME}/lib:${JRE_HOME}/lib
export IDEA_JDK_64=/usr/lib/jvm/jdk1.8
export IDEA_JDK=/usr/lib/jvm/jdk1.8
export PATH=${JAVA_HOME}/bin:$PATH
# maven
export MAVEN_HOME=/home/pmz/maven3.3.9
export PATH=${PATH}:${MAVEN_HOME}/bin
# tomcat
export TOMCAT_HOME=/opt/tomcat7
export CATALINA_HOME=/opt/tomcat7
```

### tomcat
* /opt/tomcat
* /etc/profile
* /bin/Catalina.sh java_home
* tomcat-users.xml

### idea
* 商店
* [本地服务器激活](http://blog.lanyus.com/archives/174.html)
* `/usr/local/bin/idea`
* sudo ./IntelliJIDEALicenseServer_linux_amd64
* Linux version 在 download 目录，运行直接在bin/.idea.sh
* `./Downloads/idea-IU-163.12024.16/bin/idea.sh `

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

### curl
* https://curl.haxx.se/download/
* `wget https://curl.haxx.se/download/curl-7.53.1.tar.gz`
* [curl网站开发 by ruanyf](http://www.ruanyifeng.com/blog/2011/09/curl.html)

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
