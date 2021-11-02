

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
    User youname
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

