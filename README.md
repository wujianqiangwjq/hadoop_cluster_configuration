配置一个完整的，小hadoop集群，后续还会加入hbase,hive，spark进入
````
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
	1. jdk
        2. 用户 kfk,并且配置好无密码访问
```
