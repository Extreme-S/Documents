账号密码：
admin
mynexus



解压文件

```bash
tar -zxvf nexus-3.12.1-01-unix.tar.gz -C /usr/local/
```

进入解压后的文件目录的etc文件下

```bash
cd /usr/local/nexus-3.12.1-01/etc
```

修改nexus配置文件

```bash
vim nexus-default.properties
```

配置文件内容，如果端口8081被占用要修改端口号：

```bash
## DO NOT EDIT - CUSTOMIZATIONS BELONG IN $data-dir/etc/nexus.properties
##
# Jetty section
application-port=8081
application-host=0.0.0.0
nexus-args=${jetty.etc}/jetty.xml,${jetty.etc}/jetty-http.xml,${jetty.etc}/jetty-requestlog.xml
nexus-context-path=/

# Nexus section
nexus-edition=nexus-pro-edition
nexus-features=\
 nexus-pro-feature
```

因为要用到8081端口，需要设置防火墙配置，打开8081端口

```bash
firewall-cmd --zone=public --add-port=8081/tcp --permanent
```

进入到nexus的bin目录下

```bash
cd /usr/local/nexus-3.12.1-01/bin
```

直接启动nexus会报warning，因为root用户没有权限，可以不用管，等待nexus启动

```bash
./nexus start
```

![image-20210522183526219](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210522183526219.png)

查看nexus的状态

![image-20210522224953255](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210522224953255.png)

