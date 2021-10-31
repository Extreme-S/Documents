# hadoop-2.6.0-cdh5.16.2

### core-site.xml

```
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://172.27.219.189:9000/</value>
    </property>
    <property>
        <name>hadoop.tmp.dir</name>
        <value>/usr/local/hadoop/tmp</value>
    </property>
</configuration>
```

### hdfs-site.xml

```java
<configuration>
	<property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
    <property>
        <name>dfs.permissions.enabled</name>
        <value>false</value>
    </property>
</configuration>
```

没有这项配置

```
    <property>
        <name>dfs.permission</name>
        <value>false</value>
    </property>
```

###  mapred-site.xml

```
<configuration>
    <property>
        <name>mapreduce.job.tracker</name>
        <value>172.27.219.189:9001</value>
    </property>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
</configuration>
```

###  yarn-site.xml

```
<configuration>
    <property>
        <name >yarn.resourcemanager.hostname</name>
        <value>172.27.219.189</value>
    </property>

    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>

    <!--使用Sqoop导入数据时，必须配置-->
    <property>
        <name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
        <value>org.apache.hadoop.mapred.ShuffleHandler</value>
    </property>

    <property>
        <name>yarn.resourcemanager.resource-tracker.address</name>
        <value>172.27.219.189:8025</value >
    </property>

    <property>
        <name>yarn.resourcemanager.scheduler.address</name>
        <value>172.27.219.189:8035</value>
    </property>

    <property>
        <name>yarn.resourcemanager.address</name>
        <value>172.27.219.189:8050</value>
    </property>
</configuration>
```

### hadoop-env.sh

```
export JAVA_HOME=/usr/local/jdk1.8.0_221

export HADOOP_COMMON_LIB_NATIVE_DIR="/usr/local/hadoop/lib/native/"
export HADOOP_OPTS="$HADOOP_OPTS -Djava.library.path=/usr/local/hadoop/lib/native/"
```



### 常用命令

```
// 格式化文件系统
hdfs namenode -format
// 查看文件
hadoop fs -ls
// 创建文件
hadoop fs -mkdir -p temp
// 新建input目录
hadoop fs -mkdir input
//把hadoop-env.sh文件放入input文件夹下面
hadoop fs -put hadoop-env.sh input/
//查看刚刚放入的文件
hadoop fs -ls /user/root/input
//查看文件内容
hadoop fs -cat input/hadoop-env.sh
// 下载文件
hadoop fs -get input/hadoop-env.sh hadoop-env2.sh
// 查看文件系统的所有信息
hadoop dfsadmin -report
// 删除文件夹
hadoop fs -rm -r output_wordcount
//删除文件
hadoop fs -rm input_wordcount/file1 input_wordcount/file2

//对文件append操作：
echo "<Text to append>" | hdfs dfs -appendToFile - yourHdfsPath/test.txt 
//对文件modify操作：
hdfs dfs -get yourHdfsPath/test.txt
vi test.txt #or use any other tool and modify it
hdfs dfs -put -f test.txt yourHdfsPath/test.txt

//进入安全模式
hdfs dfsadmin -safemode enter

//退出安全模式
hdfs dfsadmin -safemode leave
```

### 报错解决

```
[root@iZuf6bn9deummmgfvswsipZ hadoop]# hadoop fs -ls
21/06/12 16:36:54 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
Found 1 items
drwxr-xr-x   - root supergroup          0 2021-06-12 16:33 temp
[root@iZuf6bn9deummmgfvswsipZ hadoop]# 
```















