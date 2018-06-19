### [教程](https://linux.cn/article-4279-1.html)
### 1. 优点
* 可以实现高并发
* 部署简单
* 内存消耗少
* 跨平台

### 2. 缺点
* rewrite功能不够强大
* 模块少

### 3.1yum 安装
* [centos 6.5 nginx安装与配置]
(https://gist.github.com/ifels/c8cfdfe249e27ffa9ba1)


### 3.2编译安装

```sh
前提：Linux软件编译安装必须的依赖安装包  
 
* yum -y  install  gcc   gcc-c++  make 
* yum -y install  pcre  pcre-devel openssl  openssl-devel   zlib 

// 下载到 /home/pmz/pac 目录下
wget -P /home/pmz/pac http://nginx.org/download/nginx-1.12.2.tar.gz
// 解压 
tar xzvf nginx-1.12.2.tar.gz -C /home/pmz/pac
// 进入到解压后的根目录
cd ./nginx-1.12.2
// 检测环境
./configure --prefix=/usr/local/nginx --with-http_stub_status_module --with-http_ssl_module --with-pcre
```sh
Configuration summary
  + using system PCRE library
  + using system OpenSSL library
  + using system zlib library

  nginx path prefix: "/usr/local/nginx"
  nginx binary file: "/usr/local/nginx/sbin/nginx"
  nginx modules path: "/usr/local/nginx/modules"
  nginx configuration prefix: "/usr/local/nginx/conf"
  nginx configuration file: "/usr/local/nginx/conf/nginx.conf"
  nginx pid file: "/usr/local/nginx/logs/nginx.pid"
  nginx error log file: "/usr/local/nginx/logs/error.log"
  nginx http access log file: "/usr/local/nginx/logs/access.log"
  nginx http client request body temporary files: "client_body_temp"
  nginx http proxy temporary files: "proxy_temp"
  nginx http fastcgi temporary files: "fastcgi_temp"
  nginx http uwsgi temporary files: "uwsgi_temp"
  nginx http scgi temporary files: "scgi_temp"

```
//编译安装  
make   
make install

```
#### 参考
* [yum 和 源码包 安装的 区别](https://segmentfault.com/a/1190000007116797)
* [llinux 环境安装编译 nginx (源码安装包)](http://www.cnblogs.com/zoulongbin/p/6253568.html)


### 4. nginx 配置
* [nginx配置、虚拟主机、负载均衡和反向代理](https://www.zybuluo.com/phper/note/89391)
* [正确配置Linux系统ulimit值的方法](https://www.cnblogs.com/ibook360/archive/2012/05/11/2495405.html#undefined)
### 5. Nginx启动关闭
```shell
start nginx
nginx -s stop       快速关闭Nginx，可能不保存相关信息，并迅速终止web服务。
nginx -s quit       平稳关闭Nginx，保存相关信息，有安排的结束web服务。
nginx -s reload     因改变了Nginx相关配置，需要重新加载配置而重载。
nginx -s reopen     重新打开日志文件。
nginx -c filename   为 Nginx 指定一个配置文件，来代替缺省的。
nginx -t            不运行，而仅仅测试配置文件。nginx 将检查配置文件的语法的正确性，并尝试打开配置文件中所引用到的文件。
nginx -v            显示 nginx 的版本。
nginx -V            显示 nginx 的版本，编译器版本和配置参数。
```

### nginx 作为资源下载服务器

* 假如文件存放在：`/home/upload/images/` 目录下

* nginx.conf server部分配置如下

```yml
server {
        listen       8000;
        #listen       somename:8080;
        server_name  localhost;
        charset utf-8; # 编码

        location ~ ^/images { # url正则匹配规则： images 为前缀

            root   /home/upload; # 可以看到与location配合成最终文件存放目录， /home/upload/images/
            #index  index.html index.htm;
            autoindex on; # 打开目录浏览功能
            autoindex_exact_size on; # 显示文件大小
            autoindex_localtime on; # 显示文件时间
        }
    }

```
此外 /home/upload/images/ 的权限为 `drwxr-x--- 2 root root 36864 Nov 29 15:03`  
所以 nginx.conf user部分配置为
```yml
user  root root; # 用户，用户组 防止访问时出现 403 forbidden
```
* 启动后，访问 `http://ip:8000/images/`

## NGINX ciphers 配置
* [NGINX ciphers 配置](https://blog.csdn.net/makenothing/article/details/63768914) 
* [专业配置建议](https://cipherli.st/)

## 配置
```
client_max_body_size 20m;
```
