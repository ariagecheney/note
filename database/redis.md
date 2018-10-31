## Redis 是单进程单线程对外提供服务的

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
yum -y  install  gcc   gcc-c++  make
make test
$ make 
or make distclean
```
* 配置文件 ./redis.conf
```sh
# 密码
requirepass 

port 6379
daemonize yes
logfile /home/eversec/env/redis-4.0.11/logs/log
dir ./
```
* 启动 The binaries that are now compiled are available in the src directory. Run Redis with:
```sh
src/redis-server ./redis.conf &
src/redis-server --daemonize yes

src/redis-cli -c -h ip -p port shutdown
pkill redis-server
```
## [命令](http://redisdoc.com/)
* redis-cli -h 127.0.0.1 -p 6379 -a myRedis
* config get requirepass
* 批量模糊删除 key  
`redis-cli -p 6389 -a pwd keys "child_device:*" |xargs redis-cli -p 6389 -a pwd del`

* DBSIZE 当前数据库的 key 的数量


3. docker

```sh
# 拉取 redis 镜像
> docker pull redis
# 运行 redis 容器
> docker run --name myredis -d -p6379:6379 redis
# 执行容器中的 redis-cli，可以直接使用命令行操作 redis
> docker exec -it myredis redis-cli
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

## 数据结构

### list
Redis 的列表相当于 Java 语言里面的 LinkedList，注意它是链表而不是数组。这意味着 list 的插入和删除操作非常快，时间复杂度为 O(1)，但是索引定位很慢，时间复杂度为 O(n)，这点让人非常意外。

当列表弹出了最后一个元素之后，该数据结构自动被删除，内存被回收。

慢操作慎用：

lindex 相当于 Java 链表的get(int index)方法，它需要对链表进行遍历，性能随着参数index增大而变差


如果再深入一点，你会发现 Redis 底层存储的还不是一个简单的 linkedlist，而是称之为快速链表 quicklist 的一个结构。

首先在列表元素较少的情况下会使用一块连续的内存存储，这个结构是 ziplist，也即是压缩列表。它将所有的元素紧挨着一起存储，分配的是一块连续的内存。当数据量比较多的时候才会改成 quicklist。因为普通的链表需要的附加指针空间太大，会比较浪费空间，而且会加重内存的碎片化。比如这个列表里存的只是 int 类型的数据，结构上还需要两个额外的指针 prev 和 next 。所以 Redis 将链表和 ziplist 结合起来组成了 quicklist。也就是将多个 ziplist 使用双向指针串起来使用。这样既满足了快速的插入删除性能，又不会出现太大的空间冗余


### set

它的内部实现相当于一个特殊的字典，字典中所有的 value 都是一个值NULL。

### zset
一方面它是一个 set，保证了内部 value 的唯一性，另一方面它可以给每个 value 赋予一个 score，代表这个 value 的排序权重。它的内部实现用的是一种叫做「跳跃列表」的数据结构。
内部 score 使用 double 类型进行存储，所以存在小数点精度问题

`zrangebyscore books -inf 8.91 withscores`   
根据分值区间 (-∞, 8.91] 遍历 zset，同时返回分值。inf 代表 infinite，无穷大的意思。



## 持久化
* https://www.jianshu.com/p/bedec93e5a7b

### rdb
* RDB文件可以通过两个命令来生成：

SAVE：阻塞redis的服务器进程，直到RDB文件被创建完毕。  
BGSAVE：派生(fork)一个子进程来创建新的RDB文件，记录接收到BGSAVE当时的数据库状态，父进程继续处理接收到的命令，子进程完成文件的创建之后，会发送信号给父进程，而与此同时，父进程处理命令的同时，通过轮询来接收子进程的信号。

* 而RDB文件的载入一般情况是自动的，redis服务器启动的时候，redis服务器再启动的时候如果检测到RDB文件的存在，那么redis会自动载入这个文件。

* 如果服务器开启了AOF持久化，那么服务器会优先使用AOF文件来还原数据库状态。

* RDB是通过保存键值对来记录数据库状态的，采用copy on write的模式，每次都是全量的备份。

* BGSAVE可以在不阻塞主进程的情况下完成数据的备份。可以通过redis.conf中设置多个自动保存条件，只要有一个条件被满足，服务器就会执行BGSAVE命令。


```
# 以下配置表示的条件：
# 服务器在900秒之内被修改了1次
save 900 1
# 服务器在300秒之内被修改了10次
save 300 10
# 服务器在60秒之内被修改了10000次
save 60 10000
```
### AOF持久化（Append-Only-File）
* 在AOF持久化的文件中（可识别的纯文本），数据库会记录下所有变更数据库状态的命令（AOF日志也不是完全按客户端的请求来生成日志的），除了指定数据库的select命令，其他的命令都是来自client的，这些命令会以追加(append)的形式保存到文件中。

* 服务器配置中有一项appendfsync，这个配置会影响服务器多久完成一次命令的记录：

always：将缓存区的内容总是即时写到AOF文件中。  
everysec：将缓存区的内容每隔一秒写入AOF文件中。  
no ：写入AOF文件中的操作由操作系统决定，一般而言为了提高效率，操作系统会等待缓存区被填满，才会开始同步数据到磁盘。  
redis默认实用的是everysec。  
* redis在载入AOF文件的时候，会创建一个虚拟的client，把AOF中每一条命令都执行一遍，最终还原回数据库的状态，它的载入也是自动的。在RDB和AOF备份文件都有的情况下，redis会优先载入AOF备份文件

* AOF文件可能会随着服务器运行的时间越来越大，可以利用AOF重写的功能，来控制AOF文件的大小。AOF重写功能会首先读取数据库中现有的键值对状态，然后根据类型使用一条命令来替代前的键值对多条命令。

* AOF重写功能有大量写入操作，所以redis才用子进程来处理AOF重写。这里带来一个新的问题，由于处理重新的是子进程，这样意味着如果主线程的数据在此时被修改，备份的数据和主库的数据将会有不一致的情况发生。因此redis还设置了一个AOF重写缓冲区，这个缓冲区在子进程被创建开始之后开始使用，这个期间，所有的命令会被存两份，一份在AOF缓存空间，一份在AOF重写缓冲区，当AOF重写完成之后，子进程发送信号给主进程，通知主进程将AOF重写缓冲区的内容添加到AOF文件中。

```
#AOF 和 RDB 持久化方式可以同时启动并且无冲突。  
#如果AOF开启，启动redis时会加载aof文件，这些文件能够提供更好的保证。 
appendonly yes

# 只增文件的文件名称。（默认是appendonly.aof）  
# appendfilename appendonly.aof 
#redis支持三种不同的写入方式：  
#  
# no:不调用，之等待操作系统来清空缓冲区（30s）当操作系统要输出数据时。很快。  
# always: 每次更新数据都写入仅增日志文件。慢，但是最安全。
# everysec: 每秒调用一次。折中。
appendfsync everysec  

# 设置为yes表示rewrite期间对新写操作不fsync,暂时存在内存中,等rewrite完成后再写入.官方文档建议如果你有特殊的情况可以配置为'yes'。但是配置为'no'是最为安全的选择。
no-appendfsync-on-rewrite no  

# 自动重写只增文件。  
# redis可以自动盲从的调用‘BGREWRITEAOF’来重写日志文件，如果日志文件增长了指定的百分比。  
# 当前AOF文件大小是上次日志重写得到AOF文件大小的二倍时，自动启动新的日志重写过程。
auto-aof-rewrite-percentage 100  
# 当前AOF文件启动新的日志重写过程的最小值，避免刚刚启动Reids时由于文件尺寸较小导致频繁的重写。
auto-aof-rewrite-min-size 64mb

# 即使 BGREWRITEAOF 执行失败，也不会有任何数据丢失，因为旧的 AOF 文件在 BGREWRITEAOF 成功之前不会被修改。
重写操作只会在没有其他持久化工作在后台执行时被触发，也就是说：

# 如果 Redis 的子进程正在执行快照的保存工作，那么 AOF 重写的操作会被预定(scheduled)，等到保存工作完成之后再执行 AOF 重写。在这种情况下， BGREWRITEAOF 的返回值仍然是 OK ，但还会加上一条额外的信息，说明 BGREWRITEAOF 要等到保存操作完成之后才能执行。在 Redis 2.6 或以上的版本，可以使用 INFO 命令查看 BGREWRITEAOF 是否被预定。
# 如果已经有别的 AOF 文件重写在执行，那么 BGREWRITEAOF 返回一个错误，并且这个新的 BGREWRITEAOF 请求也不会被预定到下次执行。

```
## 对比
* AOF更安全，可将数据及时同步到文件中，但需要较多的磁盘IO，AOF文件尺寸较大，文件内容恢复相对较慢， 也更完整。
* RDB持久化，安全性较差(redis挂了，从上次RDB文件生成到Redis停机这段时间的数据全部丢掉了))，它是正常时期数据备份及 master-slave数据同步的最佳手段，文件尺寸较小，且是二进制的，恢复数度较快。

# 主从
redis支持master-slave模式，一主多从，redis server可以设置另外多个redis server为slave，从机同步主机的数据。配置后，读写分离，主机负责读写服务，从机只负责读。减轻主机的压力。redis实现的是最终会一致性，具体选择强一致性还是弱一致性，取决于业务场景。
redis 主从同步有两种方式（或者所两个阶段）：全同步和部分同步。
* 主从同步就是 RDB 文件的上传下载；主机有小部分的数据修改，就把修改记录传播给每个从机。
* Redis在master是非阻塞模式，也就是说在slave执行数据同步的时候，master是可以接受客户端的请求的，并不影响同步数据的一致性，然而在slave端是阻塞模式的，slave在同步master数据时，并不能够响应客户端的查询。

# 集群

配置
```
masterauth abc  
requirepass abc 
vim /home/pmz/install/ruby/lib/ruby/gems/2.5.0/gems/redis-4.0.2/lib/redis/client.rb

password abc
```

* [ruby](http://www.ruby-lang.org/en/downloads/)
```sh
$ ./configure --prefix=dir
$ make
$ sudo make install

gem install redis

find src/ -perm -u+x -type f -exec cp {} redis-cluster/bin/ \;

执行集群创建命令（只需要在其中一个节点上执行一次则可）

find / -name 'client.rb'

bin/redis-trib.rb create 127.0.0.1:9001 127.0.0.1:9002 127.0.0.1:9003

# --replicas 1

bin/redis-trib.rb check 127.0.0.1:9001

bin/redis-trib.rb add-node --slave --master-id add1bd94bd9bcba10497b68594c381f83df155f3 127.0.0.1:9004  127.0.0.1:9001

info

del-node 127.0.0.1:7000 `<node-id>`

redis-cli -c -p 9001

config set masterauth abc  
config set requirepass abc  
auth abc
config rewrite  
```

### 参考
* [runoob](http://www.runoob.com/redis/redis-intro.html)
* [redis 中文网](http://www.redis.cn/)
* [redis 命令](http://redisdoc.com/)
