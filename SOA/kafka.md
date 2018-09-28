# 集群搭建
## kafka 基于2.0.0


```
#配置文件 config/server.properties
broker.id=1
listeners=PLAINTEXT://:9093
log.dirs=/tmp/kafka-logs-1
zookeeper.connect=10.20.1.153:2181,10.20.1.154:2181,10.20.1.155:2181

# 命令行 All of the command line tools have additional options; running the command with no arguments will display usage information documenting them in more detail.

# 启动
./bin/kafka-server-start.sh -daemon config/server.properties

# topic

bin/kafka-topics.sh --create --zookeeper 192.168.200.67:2181 --replication-factor 3 --partitions 1 --topic my-replicated-topic

bin/kafka-topics.sh --delete --zookeeper 192.168.200.67:2181 --topic my-replicated-topic

bin/kafka-topics.sh --list --zookeeper 192.168.200.67:2181

bin/kafka-topics.sh --describe --zookeeper 192.168.200.67:2181

# p
bin/kafka-console-producer.sh --broker-list 192.168.200.67:9092 --topic my-replicated-topic
# c
bin/kafka-console-consumer.sh --bootstrap-server 192.168.200.67:9092 --topic my-replicated-topic --from-beginning

```