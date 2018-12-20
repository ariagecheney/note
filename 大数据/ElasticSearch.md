# 参考
* [Elastic Search 介绍和基本概念](https://www.jianshu.com/p/86a8c085e604)

### docker 安装
```sh

./elasticsearch -Ecluster.name=es_cluster -Enode.name=node01

docker run -d -it --name es -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch

```

## 使用

java api
节点客户端(node client)：节点客户端加入集群，但是并不存储数据，但知道数据再集群的具体位置，并且能够转发请求到对应节点上。
传输客户端(Transport client)：节点本身不加入集群，只简单发送请求给集群中的节点，这个更轻量。
两个客户端都是通过9300端口进行访问，并使用Elasticsearch传输协议进行通信(ETP:Elasticsearch Transport Protocol)，集群节点间也是使用9300端口进行通信。
基于http协议
基于HTTP协议，以JSON为数据交互格式的RESTful API,非java语言可以使用RESTful API形式与ES进行交互，使用9200端口。

curl http://127.0.0.1:9200/_cat/health?v


集群状态分为绿色(green)、黄色(yellow)和红色(red)。

绿色代表集群当前状态是好的(集群功能齐全)。
黄色代表所有数据(主分片)都可用，但是一些副本(副分片)还没有分配(集群功能齐全)。
红色代表一些数据(主分片)不可用。


### 单播、多播和广播
单播”（Unicast）、“多播”（Multicast）和“广播”（Broadcast）这三个术语都是用来描述网络节点之间通讯方式的术语。
