### 安装docker

查看docker的可安装版本

```bash
yum list | grep docker
```

Failed to start Docker Application Container ...ne

```
vim /etc/docker/daemon.json
加入下面这段话
{
	"registry-mirrors":["https://bjtzu1jb.mirror.aliyuncs.com"]
}
```

重新加载daemon.json配置

```
systemctl daemon-reload
```

重启docker服务 

```
systemctl restart docker.service
```

### 安装RabbitMQ—微服务大课

```
#拉取镜像
docker pull rabbitmq:3.8.12-management-alpine

#启动容器
docker run -d \
--hostname rabbit_host1 \
--name xd_rabbit \
-e RABBITMQ_DEFAULT_USER=admin \
-e RABBITMQ_DEFAULT_PASS=lyw123456 \
-p 15672:15672 \
-p 5672:5672 \
rabbitmq:3.8.12-management-alpine

#介绍
-d 以守护进程方式在后台运行
-p 15672:15672 management 界面管理访问端口
-p 5672:5672 amqp 访问端口
--name：指定容器名
--hostname：设定容器的主机名，它会被写到容器内的 /etc/hostname 和 /etc/hosts，作为容器主机IP的别名，并且将显示在容器的bash中

-e 参数
  RABBITMQ_DEFAULT_USER 用户名
  RABBITMQ_DEFAULT_PASS 密码
```

* 主要端口介绍

```
4369 erlang 发现口

5672 client 端通信口

15672 管理界面 ui 端口

25672 server 间内部通信口
```

### 安装RabbitMQ

搜索rabbitmq

```
docker search rabbitmq:management
```

列出当前系统存在的镜像

```
docker images
```

删除镜像，-f强制删除

```
docker rmi -f <IMAGE ID>
```

安装RabbitMQ

```
docker pull rabbitmq:management
```

运行容器：

```
docker run -d --name "myRabbitMQ" -p 5672:5672 -p 15672:15672 rabbitmq:management
```

```
 -d  后台运⾏
 -p  端⼝映射
 rabbitmq:management (格式 REPOSITORY:TAG)，如果不指定tag，默认使⽤最新的
 --name "xxx"
 列举当前运⾏的容器：docker ps
 检查容器内部信息：docker inspect 容器名称
 删除镜像：docker rmi IMAGE_NAME
 强制移除镜像不管是否有容器使⽤该镜像 增加 -f 参数，
 停⽌某个容器：docker stop 容器名称
 启动某个容器：docker start 容器名称
 移除某个容器： docker rm 容器名称 （容器必须是停⽌状态）
```

进入docker容器

```
docker exec -it 731cfe6b132f bash
```

查看RabbitMQ当前的用户列表

```
rabbitmqctl list_users
```

修改密码

```
rabbitmqctl change_password guest 'admin123'
```

### 安装mysql5.7

查找MySQL镜像：

```bash
docker search mysql
```

拉取MySQL镜像(:5.7 表示5.7版本)

```bash
docker pull mysql:5.7
```

本地创建mysql映射目录

```
mkdir -p /root/mysql/data /root/mysql/logs /root/mysql/conf
```

在/root/mysql/conf中创建 *.cnf 文件(叫什么都行)

```
touch my.cnf
```

创建容器,将数据,日志,配置文件映射到本机

```
docker run -d
-p 3306:3306 
--name mysql 
-v /root/mysql/conf:/etc/mysql/conf.d 
-v /root/mysql/logs:/logs 
-v /root/mysql/data:/var/lib/mysql 
-e MYSQL_ROOT_PASSWORD=root 
mysql:5.7
```

```
docker run -d \
-p 3306:3306 \
-v /docker/mysql/conf:/etc/mysql/conf.d \
-v /docker/mysql/data:/var/lib/mysql \
-v /docker/mysql/logs:/logs \
-e MYSQL_ROOT_PASSWORD=lyw123456 \
--privileged=true \
--name mysql mysql:5.7 \
--character-set-server=utf8mb4 \
--collation-server=utf8mb4_general_ci
```

```
run　run 是运行一个容器
-d　 表示后台运行
-p　　表示容器内部端口和服务器端口映射关联
--privileged=true　设值MySQL 的root用户权限, 否则外部不能使用root用户登陆
-v /docker/mysql/conf/my.cnf:/etc/my.cnf 将服务器中的my.cnf配置映射到docker中的/docker/mysql/conf/my.cnf配置
-v /docker/mysql/data:/var/lib/mysql　　同上,映射数据库的数据目录, 避免以后docker删除重新运行MySQL容器时数据丢失
-e MYSQL_ROOT_PASSWORD=123456　　　设置MySQL数据库root用户的密码
--name mysql　　　　 设值容器名称为mysql
mysql:5.7　　表示从docker镜像mysql:5.7中启动一个容器
--character-set-server=utf8mb4 --collation-server=utf8mb4_general_ci 设值数据库默认编码
```

查看启动日志

```
docker logs -f 
```

进入容器

```bash
docker exec -it 1cbf42c6a0db bash
```

```
mysql -u root -p
```



### 安装配置Nacos

账号：nacos
密码：lyw123456

```
docker run --env MODE=standalone --name my-nacos -d -p 8848:8848  nacos/nacos-server
```

Nacos注册中心配置mysql持久化

```
docker run -d \
-e MODE=standalone \
-e SPRING_DATASOURCE_PLATFORM=mysql \
-e MYSQL_SERVICE_HOST=8.140.36.154 \
-e MYSQL_SERVICE_PORT=3306 \
-e MYSQL_SERVICE_USER=root \
-e MYSQL_SERVICE_PASSWORD=lyw123456 \
-e MYSQL_SERVICE_DB_NAME=ec_nacos \
-p 8848:8848 \
--restart=always \
--name nacos \
nacos/nacos-server 
```



### docker常用操作

查看本地镜像

```
docker images
```

删除镜像

```
docker rmi eda0212ec0fe
```

显示所有(-a)容器列表

```
docker ps -a
```

停止容器

```
docker stop eda0212ec0fe
```

删除容器

```
docker rm eda0212ec0fe
```

进入容器

```
docker attach 44fc0f0582d9  
这种方式有缺点，当多个窗口同时使用该命令进入该容器时，所有的窗口都会同步显示。
如果有一个窗口阻塞了，那么其他窗口也无法再进行操作。
推荐下面这种
docker exec -it 1cbf42c6a0db bash
```

退出容器命令

```
exit
Ctrl+P+Q
```

Nacos绑定本地Mysql

```
docker run -d \
-e MODE=standalone \
-e SPRING_DATASOURCE_PLATFORM=mysql \
-e MYSQL_SERVICE_HOST=112.54.164.148 \
-e MYSQL_SERVICE_PORT=3306 \
-e MYSQL_SERVICE_USER=root \
-e MYSQL_SERVICE_PASSWORD=123456 \
-e MYSQL_SERVICE_DB_NAME=my_nacos \
-p 8848:8848 \
--restart=always \
--name nacos \
nacos/nacos-server 
```

查看启动日志

```
docker logs -f 
```

