### install & launch
1. win
    * 配置文件   
    redis.window.conf文件中 maxheap 设置为104857600（100M）
    * 启动server  
    `redis-server.exe redis.windows.conf`
    * 启动client  
    `redis-cli.exe -h 127.0.0.1 -p 6379 -a "password"`  
    * 运行时手动配置
    ```sh
    config get * //获取所有配置
    config get xx  
    config set xx xx
    ```

2. linux

* Download, extract and compile Redis with:
```sh
$ wget http://download.redis.io/releases/redis-4.0.10.tar.gz
$ tar xzf redis-4.0.10.tar.gz
$ cd redis-4.0.10
$ make
```
* The binaries that are now compiled are available in the src directory. Run Redis with:
```sh
$ src/redis-server
```
##  查看Redis 进程命令：  
ps -ax|grep redis  
本地 6379 端口号已被 redis 监听。  
$ netstat -an | grep 6379  
2、Redis服务的重启操作：（先pkill掉redis-server进程，再启动）  
pkill掉redis-server进程：pkill redis-server  
启动：
/home/redis-3.2.1/src/redis-server /home/redis-3.2.1/redis.conf & 
## 查看redis的版本有两种方式：
1. redis-server --version 和 redis-server -v 
得到的结果是：Redis server v=2.6.10 sha=00000000:0 malloc=jemalloc-3.2.0 bits=32

2. redis-cli --version 和 redis-cli -v
　得到的结果是：redis-cli 2.6.10
* 严格上说：通过　redis-cli 得到的结果应该是redis-cli 的版本，但是 redis-cli 和redis-server　一般都是从同一套源码编译出的。所以应该是一样的。
## redis-cli 连接操作相关的命令

默认直接连接  远程连接 -h 192.168.1.20 -p 6379  
ping：测试连接是否存活如果正常会返回pong  
echo：打印  
select：切换到指定的数据库，数据库索引号 index 用数字值指定，以 0 作为起始索引值  
quit：关闭连接（connection）  
auth：简单密码认证  

## 命令
* 批量模糊删除 key  
`redis-cli -p 6389 -a pwd keys "child_device:*" |xargs redis-cli -p 6389 -a pwd del`


## 持久化
* https://www.jianshu.com/p/bedec93e5a7b
### 参考
* [runoob](http://www.runoob.com/redis/redis-intro.html)
* [redis 中文网](http://www.redis.cn/)
* [redis 命令](http://redisdoc.com/)
