安装准备如下:

一个hadoop集群

一个zookeeper集群,这里重点是讲Hbase集群的搭建，所以默认你已经有了hadoop集群和zookeeper集群，并且已经全部运行了

我这里用的是CDH5.3.6,所以不用考虑兼容性问题，

角色分配如下：

机器一：  namenode   datanode   regionserver   hmaster     zookeeper

机器二：  datanode     regionserver     zookeeper

机器三：  datanode     regionserver     zookeeper

首先当然是上传解压，，然后修改配置文件

**hbase-env.sh**

export JAVA\_HOME=/opt/jdk1.8.0\_161                           *这里换成你自己的jdk路径

export HBASE\_MANAGES\_ZK=false                             *这里改为false是禁用hbase自带的zookeeper，使用外部的zookeeper,                                                                                                     因为zookeeper不仅要监控hbase,还要监控其他的

**hbase-site.xml**

<property>  
<name>hbase.zookeeper.property.dataDir</name>  
<value>/opt/zookeeper-3.4.5-cdh5.3.6/data/zkData</value>                    *这个是你的zookeeper集群配置文件里面dataDir的路径  
</property>  
<property>  
<name>hbase.rootdir</name>  
<value>hdfs://192.168.83.110:8020/hbase</value>  
</property>  
<property>  
<name>hbase.cluster.distributed</name>  
<value>true</value>  
</property>  
<property>  
<name>hbase.zookeeper.quorum</name>  
<value>cdh0,cdh1,cdh2</value>  
</property>

**regionserver**

cdh0

cd h1

cdh2

配置好之后就可以启动hbase集群了，当然前提是你的hadoop和zookeeper已经准备好了

bin/start-hbase.sh

![avatar](大数据/hbase/hbase集群搭建png/20180810141221125.png)

使用hbase的命令行客户端查看hbase是否能够正常运行

bin/hbase  shell

![avatar](大数据/hbase/hbase集群搭建png/20180810141331635.png)

list:   查看表

![avatar](大数据/hbase/hbase集群搭建png/20180810141502563.png)

status:    查看集群状态

![avatar](大数据/hbase/hbase集群搭建png/20180810141520278.png)

version:    查看集群版本

如果在命令行客户端中出现错误，就进入zookeeper把hbase删除，然后重新启动hbase，具体步骤如下

在zookeeper目录下

bin/zkCli.sh

ls /

rmr  hbase

![avatar](大数据/hbase/hbase集群搭建png/20180810141909237.png)

然后重新启动Hbase集群


