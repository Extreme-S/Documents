# 阿里云CentOS7+Hadoop3.1.4伪分布式集群搭建

### Hadoop 下载安装

这是大学学校的镜像，似乎wget不能用，建议去网址下载

```
wget http://mirror.bit.edu.cn/apache/hadoop/core/hadoop-3.1.4/hadoop-3.1.4.tar.gz
```

解压到opt目录下面

```
tar -zxvf hadoop-3.1.4.tar.gz -C /opt
```

进入到hadoop-3.1.4文件下，创建以下文件

```
/opt/hadoop-3.1.4/data
/opt/hadoop-3.1.4/hdfs/name
/opt/hadoop-3.1.4/hdfs/data
```

### 添加Hadoop的环境变量

编辑`/etc/profile`配置文件

```
vim /etc/profile
```

文件末尾添加如下内容   

```
HADOOP_HOME=/opt/hadoop-3.1.4
PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin
export HADOOP_HOME PATH
```

使profile配置文件生效

```
source /etc/profile
```

### 关联主机名和IP地址

用`hostname`命令查看Linux的主机名，如果能ping通说明已经关联了，阿里云默认是关联的

![image-20210607203056996](https://raw.githubusercontent.com/Extreme-S/img-bed/main/posts/image-20210607203056996.png)

`172.27.219.189`是服务器的**内网ip**

在后面的配置中`172.27.219.189`和`iZuf6bn9deummmgfvswsipZ`是等价的

修改`/opt/hadoop-3.1.4/etc/hadoop/workers`文件，将localhost改为你的主机名

![image-20210607213608037](https://raw.githubusercontent.com/Extreme-S/img-bed/main/posts/image-20210607213608037.png)



### 修改Hadoop配置文件

进入`/opt/hadoop-3.1.4/etc/hadoop`目录，修改以下配置文件

![image-20210607150252989](https://raw.githubusercontent.com/Extreme-S/img-bed/main/posts/image-20210607150252989.png)

#### 配置hadoop-env.sh

输入命令`vim hadoop-env.sh` 添加你自己的jdk安装路径和Hadoop角色配置

```
export JAVA_HOME=/usr/local/software/jdk1.8
export HDFS_NAMENODE_USER=root
export HDFS_DATANODE_USER=root
export HDFS_SECONDARYNAMENODE_USER=root
export YARN_RESOURCEMANAGER_USER=root
export YARN_NODEMANAGER_USER=root
```

#### 配置core-site.xml

Hadoop的核心组件文件是core-site.xml

文件路径`/opt/hadoop-3.1.4/etc/hadoop/core-site.xml`，添加如下配置

```配置core-site.xml
<configuration>
	<!--定义namenode地址 默认9820端口-->
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://172.27.219.189:9820</value>
    </property>
    
	<!--修改用于hadoop存储数据的默认位置-->
    <property>
        <name>hadoop.tmp.dir</name>
        <value>/opt/hadoop-3.1.4/data</value>
    </property>
</configuration>
```

xlynw 172.17.7.23

#### 配置hdfs-site.xml

Hadoop的文件系统配置文件是hdfs-site.xml

文件路径`/opt/hadoop-3.1.4/etc/hadoop/hdfs-site.xml`，添加如下配置

```
<configuration>
    <property>
        <name>dfs.namenode.name.dir</name>
        <value>/opt/hadoop-3.1.4/hdfs/name</value>
    </property>

    <property>
        <name>dfs.datanode.data.dir</name>
        <value>/opt/hadoop-3.1.4/hdfs/data</value>
    </property>
    
	<!--指定副本数，伪分布式下副本数指定为1即可-->
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
</configuration>
```

#### 配置mapred-site.xml

mapred-site.xml 指定MapReduce程序应该放在哪个资源调度集群上运行。若不指定为yarn，那么MapReduce程序就只会在本地运行而非在整个集群中运行。

文件路径`/opt/hadoop-3.1.4/etc/hadoop/mapred-site.xml`，添加配置如下

```
<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
   
    <property>
        <name>yarn.app.mapreduce.am.env</name>
        <value>HADOOP_MAPRED_HOME=/opt/hadoop-3.1.4</value>
    </property>
    <property>
        <name>mapreduce.map.env</name>
        <value>HADOOP_MAPRED_HOME=/opt/hadoop-3.1.4</value>
    </property>
    <property>
        <name>mapreduce.reduce.env</name>
        <value>HADOOP_MAPRED_HOME=/opt/hadoop-3.1.4</value>
    </property>
</configuration>
```

#### 配置yarn-site.xml

Yarn的站点配置文件是yarn-site.xml

文件路径`/opt/hadoop-3.1.4/etc/hadoop/yarn-site.xml`，添加配置如下

```
<configuration>
    <!-- 设置ResourceManager地址 -->
    <property>
        <name>yarn.resourcemanager.hostname</name>
        <value>172.27.219.189</value>
    </property>
    
    <!-- The http address of the RM web application -->
    <property>
        <name>yarn.resourcemanager.webapp.address</name>
        <value>172.27.219.189:8088</value>
    </property>
    
    <property>
    	<name>yarn.nodemanager.aux-services</name>
		<value>mapreduce_shuffle</value>
	</property>
</configuration>
```

以下是可选配置，可忽略

```
    <!-- 开启yarn.webapp.ui2 -->
    <property>
        <name>yarn.webapp.ui2.enable</name>
        <value>true</value>
    </property>
    
    <!-- 默认为true, 当虚拟机内存不够多时，容易超出虚拟机内存 -->
    <property>
        <name>yarn.nodemanager.vmem-check-enabled</name>
        <value>false</value>
        <description>enforced for containers.</description>
    </property>
```



### 配置免密登录

因为需要从主节点启动集群，所以需要实现主到从的免密登录，伪分布式集群只有一个主节点也要配置，自己也需要登陆自己

创建公钥/私钥，yes之后一路回车

```
ssh-keygen -t rsa
```

进入/root/.ssh目录，将公钥追加到authorized_keys文件中去

```
cd /root/.ssh
cat id_rsa.pub >> authorized_keys
```

> **分布式集群扩展：**
> 如果要实现从到主的免密登录，由公钥/私钥的验证机制
> 需将主节点的~/.ssh/id_rsa.pub中的公钥追加到从节点的~/.ssh/authorized_keys中，

使用`ssh 主机名/公网ip`命令验证能否免密登录，如下图说明免密登录设置成功

![image-20210608113314000](https://raw.githubusercontent.com/Extreme-S/img-bed/main/posts/image-20210608113314000.png)



### 启动Hadoop集群

在`/opt/hadoop3.1.4`目录下执行如下命令

启动前必须执行如下格式化命令，不然namenode无法启动，无法访问hdfs

```
hadoop namenode -format
```

启动集群

```
start-all.sh
```

使用`jps`命令查看当前启动的java进程，注意这5个进程要正常启动，前面的数字是进程号

如果有进程没有启动，去`/opt/hadoop-3.1.4/logs`目录下查看对应的日志解决问题

![image-20210608111555574](https://raw.githubusercontent.com/Extreme-S/img-bed/main/posts/image-20210608111555574.png)

根据上面 NameNode 和 ResourceManager的进程号，用`netstat+grep`命令查询进程的对应端口

![image-20210608111929247](https://raw.githubusercontent.com/Extreme-S/img-bed/main/posts/image-20210608111929247.png)

知道了对应的端口，用`公网ip:端口`的格式就可以访问了，

当然，还要配置阿里云的安全组开放对应的端口，如果服务器开了防火墙，防火墙的端口也要打开，这里就不再赘述了

Namenode information: http://8.133.166.228:9870/

![image-20210607213946902](https://raw.githubusercontent.com/Extreme-S/img-bed/main/posts/image-20210607213946902.png)

All Applications: http://8.133.166.228:8088/

![image-20210607213934693](https://raw.githubusercontent.com/Extreme-S/img-bed/main/posts/image-20210607213934693.png)

### Hadoop常用命令：

```
停止所有的Hadoop守护进程。
stop-all.sh

启动所有的Hadoop守护进程
start-all.sh
```

























