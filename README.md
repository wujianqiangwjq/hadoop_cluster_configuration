```
配置一个完整的，小hadoop集群，后续还会加入hbase,hive，spark进入
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
        3. ulimit 设置,请不要乱设值，否则系统可莪能无法启动。
            kfk  -       nofile  32768
            kfk  -       nproc   32000
        4. 需要设置权限/etc/sudoers,我在Docker上跑，可能与生产环境有点不同.
             kfk ALL=(root)NOPASSWS:ALL 
          
```
```
hbase 配置简单一点，但是hbase底层是依赖HDFS，所以hbase启动之前需要启动hdfs集群
hbase包，里面含有zookeeper如果不想使用它自带的zookeeper需要在hbase-env.sh配置
hbase-env.sh中配置:
     export JAVA_HOME=/usr/java/jdk
     export HBASE_MANAGES_ZK=false

 hbase plan:
            master                    slave1                   slave2
            hmaster                   hmaster                  hregionserver
            hregionserver             hregionserver
            zookeeper                 zookeeper                zookeeper
hmaster 就是master服务
hregionserver 就是regionserver服务
```
