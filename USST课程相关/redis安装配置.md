### 安装redis

配置文件路路径：/usr/local/redis6

```
#安装gcc
yum install -y gcc-c++ autoconf automake

#centos7 默认的 gcc 默认是4.8.5,版本小于 5.3 无法编译,需要先安装gcc新版才能编译
gcc -v

#升级新版gcc，并安装相关依赖
yum -y install centos-release-scl
yum -y install devtoolset-9-gcc devtoolset-9-gcc-c++ devtoolset-9-binutils

#scl enable devtoolset-9 bash  
#配置gcc版本永久生效
echo "source /opt/rh/devtoolset-9/enable" >>/etc/profile 

#将redis解压到/usr/local目录下面
tar -zxvf redis-6.2.1.tar.gz -C /usr/local/

#改名
mv redis-6.2.1 redis6

#进入到redis6目录下，编译redis
make

#安装到指定目录
mkdir -p /usr/local/redis
make PREFIX=/usr/local/redis install
```

### 启动redis

启动服务端，进入到 /usr/local/redis/bin 目录下面，输入

```bash
./redis-server
```

![image-20210524162410832](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210524162410832.png)

启动客户端，新建一个连接进入到 /usr/local/redis/bin 目录下输入

```
./redis-cli
```

![image-20210524162934594](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210524162934594.png)

### redis6自定义配置密码及守护进程

mkdir创建目录

- 日志 /usr/local/redis/log
- 数据 /usr/local/redis/data
- 配置文件 /usr/local/redis/conf

![image-20210524164353206](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210524164353206.png)

进入conf文件创建redis.conf文件并编辑加入下面内容

```
touch redis.conf
vim redis.conf
```

```
#任何ip可以访问
bind 0.0.0.0

#守护进程
daemonize yes

#密码
requirepass 123456

#日志文件
logfile "/usr/local/redis/log/redis.log"

#持久化文件名称
dbfilename xdclass.rdb

#持久化文件存储路径
dir /usr/local/redis/data

#持久化策略, 10秒内有个1个key改动，执行快照
save 10 1
```

去到/usr/local/redis/bin目录下指定配置文件启动redis-server

```
cd /usr/local/redis/bin
./redis-server ../conf/redis.conf
```

查看redis启动日志

```
tail -f -n 10 /usr/local/redis/log/redis.log
```

![image-20210524170655155](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210524170655155.png)

启动redis-cli客户端

```
./redis-cli -a 123456
```

### redis修改密码

查看使用6379这个端口的进程，并kill掉这个进程

```
lsof -i:6379
```

去到redis/conf 目录下面 修改 redis.conf文件,更改密码

去到redis/bin 目录下面 重新启动redis

```
./redis-server ../conf/redis.conf
```

