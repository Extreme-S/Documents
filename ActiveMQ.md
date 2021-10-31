# ActiveMQ 安装

### 前置环境：

- jdk环境变量配置
- 阿里云配置服务器安全组规则

### activemq安装配置

下载activemq

``` bash
wget http://archive.apache.org/dist/activemq/5.15.7/apache-activemq-5.15.7-bin.tar.gz
```

解压：

```bash
tar -xzvf apache-activemq-5.15.7-bin.tar.gz
```

```bash
I:我的总结： 
tar -xzvf .tar.gz
tar [-cxtzjvfpPN] 文件与目录 ....
参数：
-c ：建立一个压缩文件的参数指令(create 的意思)；
-x ：解开一个压缩文件的参数指令！
-t ：查看 tarfile 里面的文件！
特别注意，在参数的下达中， c/x/t 仅能存在一个！不可同时存在！
因为不可能同时压缩与解压缩。
-z ：是否同时具有 gzip 的属性？亦即是否需要用 gzip 压缩？
-j ：是否同时具有 bzip2 的属性？亦即是否需要用 bzip2 压缩？
-v ：压缩的过程中显示文件！这个常用，但不建议用在背景执行过程！
-f ：使用档名，请留意，在 f 之后要立即接档名喔！不要再加参数！
```

赋权限

```
chmod -u+rwx apache-activemq-5.12.0
```

进入bin目录 启动activemq

```bash
./activemq start
```

```bash
启动
./activemq start
重启
./activemq restart
停止
./activemq stop
查看状态
./activemq status
```

启动防火墙

```bash
systemctl start firewalld
```

```bash
Linux firewalld的基本使用
启动： systemctl start firewalld
关闭： systemctl stop firewalld
查看状态： systemctl status firewalld 
开机禁用  ： systemctl disable firewalld
开机启用  ： systemctl enable firewalld
防火墙配置文件路径 /etc/firewalld/firewalld.conf
```



编辑防火墙（centos7版本对防火墙进行了加强,不再使用原来的iptables,启用firewall）

public指作用域，80/tcp指端口号/访问类型，--permanent指永久生效

```bash
firewall-cmd --zone=public --add-port=8161/tcp --permanent
```



查看端口占用情况,activemq默认端口号 8161

```bash
netstat -tunlp | grep 8161
```

```bash
-t或--tcp 显示TCP传输协议的连线状况。
-u或--udp 显示UDP传输协议的连线状况。
-n或--numeric 直接使用IP地址，而不通过域名服务器。
-l或--listening 显示监控中的服务器的Socket。
-p或--programs 显示正在使用Socket的程序识别码和程序名称。
```





