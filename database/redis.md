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
1. 查看Redis 进程命令：
ps -ax|grep redis
本地 6379 端口号已被 redis 监听。
$ netstat -an | grep 6379
2、Redis服务的重启操作：（先pkill掉redis-server进程，再启动）
pkill掉redis-server进程：pkill redis-server
启动：
/home/redis-3.2.1/src/redis-server /home/redis-3.2.1/redis.conf &     



### 参考
* [runoob](http://www.runoob.com/redis/redis-intro.html)
* [redis 中文网](http://www.redis.cn/)
* [redis 命令](http://redisdoc.com/)
