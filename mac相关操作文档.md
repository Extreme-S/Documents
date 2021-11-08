

# mac常用快捷键

mac全屏模式下不能显示其他窗口

- Command-H：隐藏最前面的应用窗口
- Option-Command-H：只显示最前面的应用窗口，隐藏其他所有
- Command-M：最小化最前面的应用窗口
- Option-Command-M：最小化最前面的应用的所有窗口
- Command-W：关闭最前面的窗口
- Control-Command-F：全屏 
- Control-Space：搜索



# 代理配置

终端里面下载软件用这个代理，需要再配置

```bash
type enproxy
export http_proxy=http://10.110.63.27:8080
export https_proxy=http://10.110.63.27:8080 
```

![image-20211101161556540](/Users/yingwei.luo/Library/Application Support/typora-user-images/image-20211101161556540.png)

# 安装openJDK8

安装brew

```
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"
```

项目中使用jdk8，在mac中安装非常简便，使用命令即可安装:

```bash
brew install openjdk@8
```

----

先更新下库

```
brew tap AdoptOpenJDK/openjdk
```


搜索

```
 brew search /adoptopenjdk/ 

 ==> Casks
adoptopenjdk                  adoptopenjdk11                adoptopenjdk11-openj9        adoptopenjdk11-openj9-large   adoptopenjdk12-jre            adoptopenjdk12-openj9-jre       adoptopenjdk8-jre             adoptopenjdk8-openj9-jre      adoptopenjdk9    adoptopenjdk10                adoptopenjdk11-jre            adoptopenjdk11-openj9-jre     adoptopenjdk12                adoptopenjdk12-openj9         adoptopenjdk8                 adoptopenjdk8-openj9          adoptopenjdk8-openj9-large[/code]
```

选择一个版本安装

```
brew install adoptopenjdk8
```



# 安装并配置maven的环境变量

下载Maven：http://maven.apache.org/download.cgi

![image-20211102093341851](/Users/yingwei.luo/Library/Application Support/typora-user-images/image-20211102093341851.png)

其中各个版本的意思：

- Binary：编译之后的二进制文件；cd 

- Source：表示可以查看源代码的，比Binary大一点；

- tar.gz archive：Linux、macOS系统使用；

- zip archive：windows系统使用；

这里我们下载Binary tar.gz archive 版本就可以了，下载解压到指定目录

```
tar zxvf apache-maven-3.8.3-bin.tar.gz -C ~/soft
```

打开用户目录下的bash_profile文件。一定要用sudo，否则没权限保存文件。

```
sudo vi ~/.bash_profile
```

配置maven的环境变量：

```
export M2_HOME=/Users/yingwei.luo/soft/apache-maven-3.8.3
export PATH=$PATH:$M2_HOME/bin
```

：wq 保存文件，执行如下命令使配置生效：

```
source ~/.bash_profile
mvn -v //查看配置是否成功
```

![image-20211102100144453](/Users/yingwei.luo/Library/Application Support/typora-user-images/image-20211102100144453.png)



# WorkBench安装配置

最新版报错，公司的mac更新系统没权限，去下了老版本8.0.20

```
**应用程序“****MySQLWorkbench****”的这个版本不能与此版本的****macOS****配合使用。**
```



```bash
Host 10.10.*
    ProxyCommand /usr/local/Cellar/corkscrew/2.0/bin/corkscrew proxy.nioint.com 8080   %h %p
    User yingwei.luo
    Port 22
    IdentityFile ~/.ssh/id_rsa
    ServerAliveInterval 60
    TCPKeepAlive=yes

Host 10.110.*
    ProxyCommand /usr/local/Cellar/corkscrew/2.0/bin/corkscrew proxy.nioint.com 8080   %h %p
    User yingwei.luo
    Port 22
    IdentityFile ~/.ssh/id_rsa
    ServerAliveInterval 60
    TCPKeepAlive=yes

Host 10.132.*
    ProxyCommand /usr/local/Cellar/corkscrew/2.0/bin/corkscrew proxy.nioint.com 8080   %h %p
    User yingwei.luo
    Port 22
    IdentityFile ~/.ssh/id_rsa
    ServerAliveInterval 60
    TCPKeepAlive=yes


Host 10.*
    ProxyCommand /usr/local/Cellar/corkscrew/2.0/bin/corkscrew proxy.nioint.com 8080   %h %p
    User yingwei.luo
    Port 2222
    IdentityFile ~/.ssh/id_rsa
    ServerAliveInterval 60
    TCPKeepAlive=yes
```

# SpringBoot启动参数

```
--spring.config.location=/Users/yingwei.luo/Desktop/pe-galaxy-sirius/src/main/resources/
--logging.config=file:/Users/yingwei.luo/Desktop/pe-galaxy-sirius/src/main/resources/logback.xml
```

# Linux环境变量

```
// 编辑配置文件
vim /etc/profile
。。。。。。
//配置完成后使生效
source /etc/profile
```



```
JAVA_HOME=/usr/local/software/jdk1.8
CLASSPATH=$JAVA_HOME/lib/
PATH=$PATH:$JAVA_HOME/bin
export PATH JAVA_HOME CLASSPATH
```



# Zookeeper与Kafka安装配置

Linux环境下Zookeeper和Kafka安装启动**

- 安装Zookeeper (默认2181端口)

  - 默认配置文件zookeeper/config/zoo.cfg

  - 启动zk

    ```
    bin/zkServer.sh start
    ```

  - 安装lsof，查看端口占用情况

    ```
    yum install -y lsof
    
    lsof -i:2181
    ```

- 安装Kafka (默认 9092端口)

  - config目录下 server.properties

  ```
  #标识broker编号，集群中有多个broker，则每个broker的编号需要设置不同
  broker.id=0
  
  #修改下面两个配置 ( listeners 配置的ip和advertised.listeners相同时启动kafka会报错)
  #listeners(内网Ip)
  listeners=PLAINTEXT://172.19.132.214:9092
  #advertised.listeners(公网ip)
  advertised.listeners=PLAINTEXT://47.100.86.147:9092
  
  #修改zk地址,默认地址
  zookeeper.connection=localhost:2181
  ```

  - bin目录启动

  ```
  #启动
  ./kafka-server-start.sh  ../config/server.properties &
  
  #停止
  kafka-server-stop.sh
  ```

  - 创建topic

  ```
  ./kafka-topics.sh --create \
  --zookeeper 47.100.86.147:2181 \
  --replication-factor 1 \
  --partitions 1 \
  --topic xdclass-topic
  ```

  - 查看topic

  ```
  ./kafka-topics.sh --list --zookeeper 47.100.86.147:2181
  ```

Linux环境下daemon守护进程运行Kafka**

- kafka如果直接启动信息会打印在控制台,如果关闭窗口，kafka随之关闭
- 进入到kafka的bin目录，以守护进程方式启动

```
./kafka-server-start.sh -daemon ../config/server.properties &
```

#  Kafka生产者发送消息和消费者消费消息命令

- 创建topic

  ```
  ./kafka-topics.sh --create \
  --zookeeper 47.100.86.147:2181 \
  --replication-factor 1 \
  --partitions 2 \
  --topic my-topic
  ```

- 查看topic

  ```
  ./kafka-topics.sh --list --zookeeper 47.100.86.147:2181
  ```

- 生产者发送消息

  ```
  ./kafka-console-producer.sh --broker-list 47.100.86.147:9092  --topic version1-topic
  ```

- 消费者消费消息 ( --from-beginning：会把主题中以往所有的数据都读取出来, 重启后会有这个重复消费)

  ```
  ./kafka-console-consumer.sh \
  --bootstrap-server 47.100.86.147:9092 \
  --from-beginning \
  --topic xdclass-topic
  ```

- 删除topic

  ```
  ./kafka-topics.sh \
  --zookeeper 47.100.86.147:2181 \
  --delete \
  --topic t1
  ```

- 查看broker节点topic状态信息

  ```
  ./kafka-topics.sh --describe --zookeeper 47.100.86.147:2181  --topic xdclass-topic
  ```

  

# Kafka消费者组配置实现点对点消费模型

- 编辑消费者配置（确保同个名称group.id一样）
  - 编辑 config/consumer.properties
- 创建topic, 1个分区

```
./kafka-topics.sh --create \
--zookeeper 47.100.86.147:2181 \
--replication-factor 1 \
--partitions 2 \
--topic t1
```

- 查看topic 列表

```
./kafka-topics.sh --list --zookeeper 47.100.86.147:2181
```

- 指定配置文件启动 两个消费者

```
./kafka-console-consumer.sh \
--bootstrap-server 47.100.86.147:9092 \
--from-beginning \
--topic t1 \
--consumer.config ../config/consumer.properties
```

- 现象
  - 只有一个消费者可以消费到数据，一个分区只能被同个消费者组下的某个消费者进行消费



# Kafka消费者组配置实现发布订阅消费模型

- 编辑消费者配置（确保group.id 不一样）
  - 编辑 config/consumer-1.properties
  - 编辑 config/consumer-2.properties
- 创建topic, 2个分区

```
./kafka-topics.sh --create \
--zookeeper 47.100.86.147:2181 \
--replication-factor 1 \
--partitions 2 \
--topic t2 
```

- 查看topic 列表

```
./kafka-topics.sh --list --zookeeper 47.100.86.147:2181
```

- 连接生产者

```
./kafka-console-producer.sh --broker-list 47.100.86.147:9092  --topic t2
```

- 指定配置文件启动 两个消费者

```
./kafka-console-consumer.sh \
--bootstrap-server 47.100.86.147:9092 \
--from-beginning \
--topic t2 \
--consumer.config ../config/consumer-1.properties

./kafka-console-consumer.sh \
--bootstrap-server 47.100.86.147:9092 \
--from-beginning \
--topic t2 \
--consumer.config ../config/consumer-2.properties
```

- 现象
  - 两个不同消费者组的节点，都可以消费到消息，实现发布订阅模型

 

![Untitled Diagram](https://file.xdclass.net/note/2021/kafka/img/Untitled%20Diagram.png)



# Kafka数据存储流程、原理、LEO+HW讲解

- **Partition**

  - topic物理上的分组，一个topic可以分为多个partition，每个partition是一个有序的队列

  - 是以文件夹的形式存储在具体Broker本机上

    ![image-20210427181035021](https://file.xdclass.net/note/2021/kafka/img/image-20210427181035021.png)

- **LEO（LogEndOffset）**

  - 表示每个partition的log最后一条Message的位置。

     

- **HW（HighWatermark）**

  - 表示partition各个replicas数据间同步且一致的offset位置，即表示allreplicas已经commit的位置
  - HW之前的数据才是Commit后的，对消费者才可见
  - ISR集合里面最小leo

![image-20210427214131614](https://file.xdclass.net/note/2021/kafka/img/image-20210427214131614.png)

- **offset**：
  - 每个partition都由一系列有序的、不可变的消息组成，这些消息被连续的追加到partition中
  - partition中的每个消息都有一个连续的序列号叫做offset，用于partition唯一标识一条消息
  - 可以认为offset是partition中Message的id

 

- **Segment**：每个partition又由多个segment file组成；
  - segment file 由2部分组成，分别为index file和data file（log file），
  - 两个文件是一一对应的，后缀”.index”和”.log”分别表示索引文件和数据文件
  - 命名规则：partition的第一个segment从0开始，后续每个segment文件名为上一个segment文件最后一条消息的offset+1

 

- Kafka高效文件存储设计特点：
  - Kafka把topic中一个parition大文件分成多个小文件段，通过多个小文件段，就容易定期清除或删除已经消费完文件，减少磁盘占用。
  - 通过索引信息可以快速定位message
  - producer生产数据，要写入到log文件中，写的过程中一直追加到文件末尾，为顺序写，官网数据表明。同样的磁盘，顺序写能到600M/S，而随机写只有100K/S



# SpringBoot2.x整合Kafka客户端+adminApi单元测试

- 单元测试配置客户端+创建topic

```
/**
 * 设置admin 客户端
 * @return
 */
public static AdminClient initAdminClient(){
    Properties properties = new Properties();
    properties.setProperty(AdminClientConfig.BOOTSTRAP_SERVERS_CONFIG,"112.74.55.160:9092");

    AdminClient adminClient = AdminClient.create(properties);
    return adminClient;
}

  //创建
    @Test
    public  void createTopic() {
        AdminClient adminClient = initAdminClient();
        // 2个分区，1个副本
        NewTopic newTopic = new NewTopic(TOPIC_NAME, 2 , (short) 1);

        CreateTopicsResult createTopicsResult = adminClient.createTopics(Arrays.asList(newTopic));
        //future等待创建，成功不会有任何报错，如果创建失败和超时会报错。
        try {
            createTopicsResult.all().get();
        } catch (Exception e) {
            e.printStackTrace();
        }
        System.out.println("创建新的topic");

    }
```

- 查看topic

```
./kafka-topics.sh --list --zookeeper 47.100.86.147:2181
```

- 查看broker节点topic状态信息

```
./kafka-topics.sh --describe \
--zookeeper 47.100.86.147:2181  \
--topic demo-kafka-topic
```



# Kafka使用JavaAPI-AdminClient创建和列举topic

- list列举

```
    //获取
    @Test
    public  void listTopic() throws ExecutionException, InterruptedException {
        AdminClient adminClient = initAdminClient();

        //是否查看内部的topic,可以不用
        ListTopicsOptions options = new ListTopicsOptions();
        options.listInternal(true);

        ListTopicsResult listTopics = adminClient.listTopics(options);
        Set<String> topics = listTopics.names().get();
        for (String topic : topics) {
            System.err.println(topic);

        }
    }

```

- 删除

```
    //删除
    @Test
    public  void delTopicTest() {
        AdminClient adminClient = initAdminClient();
        DeleteTopicsResult deleteTopicsResult = adminClient.deleteTopics(Arrays.asList("xdclass-sp11-topic"));
        try {
            deleteTopicsResult.all().get();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
```

# Kafka原理之producer到broker的消息分发策略

- 生产者发送到broker里面的流程是怎样的呢，一个 topic 有多个 partition分区，每个分区又有多个副本
  - 如果指定Partition ID，则PR被发送至指定Partition (ProducerRecord)
  - 如果未指定Partition ID，但指定了Key, PR会按照hash(key)发送至对应Partition
  - 如果未指定Partition ID也没指定Key，PR会按照默认 round-robin轮训模式发送到每个Partition
    - 消费者消费partition分区默认是range模式
  - 如果同时指定了Partition ID和Key, PR只会发送到指定的Partition (Key不起作用，代码逻辑决定)
  - 注意：Partition有多个副本，但只有一个replicationLeader负责该Partition和生产者消费者交互
- 生产者到broker发送流程
  - Kafka的客户端发送数据到服务器，不是来一条就发一条，会经过内存缓冲区（默认是16KB），通过KafkaProducer发送出去的消息都是先进入到客户端本地的内存缓冲里，然后把很多消息收集到的Batch里面，再一次性发送到Broker上去的，这样性能才可能题高

# Kafka原理之Consumer消费者机制和分区策略

- 消费者根据什么模式从broker获取数据的？
  - 为什么是pull模式，而不是broker主动push？
    - 消费者采用 pull 拉取方式，从broker的partition获取数据
    - pull 模式则可以根据 consumer 的消费能力进行自己调整，不同的消费者性能不一样
      - 如果broker没有数据，consumer可以配置 timeout 时间，阻塞等待一段时间之后再返回
    - 如果是broker主动push，优点是可以快速处理消息，但是容易造成消费者处理不过来，消息堆积和延迟。
- 消费者从哪个分区进行消费？
  - 一个 topic 有多个 partition，一个消费者组里面有多个消费者，那是怎么分配过
    - 一个主题topic可以有多个消费者，因为里面有多个partition分区 ( leader分区)
    - 一个partition leader可以由一个消费者组中的一个消费者进行消费
    - 一个 topic 有多个 partition，所以有多个partition leader，给多个消费者消费，那分配策略如何？



























