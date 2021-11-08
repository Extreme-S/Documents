解压到/usr/local/目录下（约定俗成的 usr/local 是软件安装位置）

``` bash
tar -zxvf apache-maven-3.5.3-bin.tar.gz -C /usr/local/
tar -zxvf jdk-8u221-linux-x64.tar.gz -C /usr/local/
```

进入/usr/local，`pwd`复制这条路径

修改环境变量

```bash
vim /etc/profile
```

按i进入insert模式，文件末尾加上如下内容

```bash
MAVEN_HOME=/usr/local/apache-maven-3.5.3
PATH=$PATH:$MAVEN_HOME/bin
export MAVEN_HOME PATH

JAVA_HOME=/usr/local/jdk1.8.0_221
PATH=$PATH:$JAVA_HOME/bin
export JAVA_HOME PATH
```

按ESC退出insert模式，再输入`：wq`保存文件

更新服务器配置文件

```bash
source /etc/profile
```

查看maven版本, 注意 -v 是小写

```bash
mvn -v
```

