# zookeeper
* [ZooKeeper 概念](https://juejin.im/post/5b970f1c5188255c865e00e7#heading-15)
* [详解分布式协调服务 ZooKeeper](https://draveness.me/zookeeper-chubby)

## 安装
* [down](https://www.apache.org/dyn/closer.cgi/zookeeper/)
* 配置
```
tickTime=2000
initLimit=10
syncLimit=5
dataDir=/opt/zookeeper/zkdata
dataLogDir=/opt/zookeeper/zkdatalog
clientPort=12181
server.1=192.168.7.100:12888:13888
server.2=192.168.7.101:12888:13888
server.3=192.168.7.107:12888:13888
#server.1 这个1是服务器的标识也可以是其他的数字， 表示这个是第几号服务器，用来标识服务器，这个标识要写到快照目录下面myid文件里
#192.168.7.107为集群里的IP地址，第一个端口是master和slave之间的通信端口，默认是2888，第二个端口是leader选举的端口，集群刚启动的时候选举或者leader挂掉之后进行新的选举的端口默认是3888
```
* 命令 
```  
启动   
./zkServer.sh start 
./zkServer.sh stop 
检查服务器状态  
./zkServer.sh status

./bin/zkCli.sh -server 127.0.0.1:12181
help
ls /
get /path
quit

``` 
# java 超时重试

# java nio aio 零拷贝