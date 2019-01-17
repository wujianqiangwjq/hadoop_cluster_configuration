```
配置一个完整的，小hadoop集群，后续还会加入hbase,hive，spark进入
download: http://archive.cloudera.com/cdh5/cdh/5
plan:
               master                  slave1                       slave2
HDFS           namenode                namenode
               datanode                datanode                     datanode
               journalnode             journalnode                  journalnode
               zkfc                    zkfc
zookeeper      zookeeper               zookeeper                   zookeeper

yarn           resourcemanager         resourcemanager 
               nodemanager             nodemanager                nodemanager
准备环境:
	1. jdk8
        2. 用户 kfk,并且配置好无密码访问
        3. ulimit 设置,请不要乱设值，否则系统可莪能无法启动。
            kfk  -       nofile  32768
            kfk  -       nproc   32000
        4. 需要设置权限/etc/sudoers,我在Docker上跑，可能与生产环境有点不同.
             kfk ALL=(root)NOPASSWS:ALL 
          
```
```
hbase 配置简单一点，但是hbase底层是依赖HDFS，所以hbase启动之前需要启动hdfs集群
hbase包，里面含有zookeeper如果不想使用它自带的zookeeper需要在hbase-env.sh配置
注意JDK的版本，hbase 支持JDK8，JDK7不支持
download: http://archive.cloudera.com/cdh5/cdh/5
hbase-env.sh中配置:
     export JAVA_HOME=/usr/java/jdk8
     export HBASE_MANAGES_ZK=false

 hbase plan:
            master                    slave1                   slave2
            hmaster                   hmaster                  hregionserver
            hregionserver             hregionserver
            zookeeper                 zookeeper                zookeeper
hmaster 就是master服务
hregionserver 就是regionserver服务
```
```
hive 配置，Hive是facebook开源的，用于解决海量结构化数据的统计,它提供了SQL查询功能
 hive架构:
         1. 底层的HDFS
         2. yarn 的mapreduce 计算(sql语句执行)
         3. meta store 可以是mysql，也可以是默认的
download: http://archive.cloudera.com/cdh5/cdh/5
 配置:
      1. 在hive-env.sh中配置:
	HIVE_CONF_DIR
        HADOOP_HOME
      2. HDFS文件系统中创建文件:
        hadoop fs -mkdir  /tmp
        hadoop fs -chmod g+w   /tmp
 	hadoop fs -mkdir  -p /user/hive/warehouse
	hadoop fs -chmod g+w   /user/hive/warehouse
启动hive进入
```
```
download: http://spark.apache.org/
spark 配置:
        注意Hadoop版本的选择
        配置主要是spark-env.sh和slaves
        spark 的调度器可以有多种选择
           1. yarn
           2. mesos
           新版本可能还有其它的
        选择yarn的话，在spark-env.sh必须配置HADOOP_HOME,HADOOP_CONF_DIR
```     
```
kafka 是一种分布式消息框架，类似的还有rabbitmq,性能上kafka高于rabbitmq但是kafka不能保证数据的完整，rabbitmq可以它是一种持久化的消息队列框架
 download:   https://archive.apache.org/dist
 配置也很简单:
    1. server.properties文件:
    	1.1 broker.id = 0	 每台机器配置不一样
        1.2 zookeeper配置        配置zookeeper，可以使用现有的zookeeper
 启动服务:
   bin/kafka-server-start.sh config/server.properties
 创建topic: 
   bin/kafka-topics.sh --create --zookeeper master:2181,slave1:2181,slave2:2181 --replication-factor 1 --partitions 1    --topic test
 查看topic:
   bin/kafka-topics.sh --list --zookeeper localhost:2181
 生产者:
   bin/kafka-console-producer.sh --broker-list master:9092,slave1:9092,slave2:9092 --topic test
 消费者:
   bin/kafka-console-consumer.sh --bootstrap-server master:9092,slave1:9092,slave2:9092 --topic test --from-beginning
``` 
