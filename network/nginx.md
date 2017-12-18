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
* [yum 和 源码包 安装的 区别](https://segmentfault.com/a/1190000007116797)

### 4. nginx 配置
* [nginx配置、虚拟主机、负载均衡和反向代理](https://www.zybuluo.com/phper/note/89391)
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