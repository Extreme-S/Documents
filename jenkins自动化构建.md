### jenkins命令相关

安装路径：

```
/usr/local/apache-tomcat-9.0.8/webapps
```

jenkins启动，因为是webapps下的war包，启动tomcat即可

```
/usr/local/apache-tomcat-9.0.8/bin/startup.sh
```



### Linux安装配置git环境

下载安装git

```bash
yum install git	//下载安装git
git version		//查看git版本
```

配置git的用户名和邮箱

```bash
git config --global user.name "Extreme-S"
git config --global user.email "1716224950@qq.com"
```

 生成密钥文件，一路回车，默认生成在/root/.ssh文件夹下 

```bash 
ssh-keygen -t rsa -C "1716224950@qq.com"
```

查看公钥内容

```bash
cat /root/.ssh/id_rsa.pub
```

![image-20210529181916528](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210529181916528.png)

将git证书配置到github上，保证linux服务器能通过ssh与github连通

![image-20210529181456406](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210529181456406.png)

![image-20210529181810959](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210529181810959.png)

验证Linux上的git配置是否成功，出现下图那句话说明配置成功

```bash
ssh git@github.com
```

![image-20210529182158542](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210529182158542.png)

### jenkins相关插件安装

去到这个页面下进行插件安装：Dashboard->系统管理->插件管理

**Generic Webhook Trigger 插件：**
本地push代码到github上后，触发Webhook，jenkins自动执行构建。

![image-20210530120714481](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210530120714481.png)

**GitHub Plugin 插件：**

![image-20210530120633691](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210530120633691.png)

### jenkins配置新建节点

将你的Linux服务器注册到 Jenkins 的节点上，Dashboard->系统管理->节点管理

![image-20210529182922188](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210529182922188.png)

配置以下信息，保存即可

![img](C:\Users\Administrator\Desktop\img.png)

刷新一下节点列表，可以看到，节点myCentOS7已经连接上了

![image-20210529204704415](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210529204704415.png)

### jenkins+GitHub联动配置

GitHub仓库分支如果更新很频繁，那么每次更新都要手动重新构建，这样就很麻烦。
为了发挥 懒人精神，这里配置一下由分支的push事件触发Webhook让jenkins自动构建

先去 GitHub 创建 access token，相当于生成一把钥匙，jenkins拿着这把钥匙就可以访问你的GitHub了

![image-20210530115604728](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210530115604728.png)

勾选如下配置，然后点击 Generate Generate，一定要保存好这个生成的token，它只显示一次。

![img2](C:\Users\Administrator\Desktop\img2.png)

然后进入 系统管理->系统配置 页面配置GitHub服务器相关信息

![image-20210530121011730](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210530121011730.png)

配置GitHub服务器，一定要勾选管理 Hook



![image-20210530122650853](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210530122650853.png)

添加凭据信息，输入刚刚生成的GitHub Access Token

![image-20210530122415813](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210530122415813.png)

配置好后，选中该凭据，点击连接测试，出现下面这句话说明能成功连接，保存配置

![image-20210530123349249](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210530123349249.png)
