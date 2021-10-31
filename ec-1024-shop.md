Docker安装和部署容器化Redis

```
#依次运行以下命令添加yum源
yum update
yum install epel-release -y
yum clean all
yum list


#安装并运行Docker。
yum install docker-io -y
systemctl start docker

#检查安装结果。
docker info

#启动使用Docker
systemctl start docker     #运行Docker守护进程
systemctl stop docker      #停止Docker守护进程
systemctl restart docker   #重启Docker守护进程


#修改镜像仓库
vim /etc/docker/daemon.json
#改为下面内容，然后重启docker
{
"debug":true,"experimental":true,
"registry-mirrors":["https://pb5bklzr.mirror.aliyuncs.com","https://hub-mirror.c.163.com","https://docker.mirrors.ustc.edu.cn"]
}

#查看信息
docker info
```

- docker部署redis 并配置密码
  - 如果访问不了，记得看防火墙/网络安全组端口是否开放
  - 源码安装redis的话默认不能远程访问
  - docker安装redis可以远程访问

```
docker run -itd --name xdclass-redis -p 8000:6379 redis --requirepass 123456
```

| Options | Mean                                               |
| ------- | -------------------------------------------------- |
| -i      | 以交互模式运行容器，通常与 -t 同时使用；           |
| -t      | 为容器重新分配一个伪输入终端，通常与 -i 同时使用； |
| -d      | 后台运行容器，并返回容器ID；                       |





## 邮箱/手机验证码防刷设计方案







## 常见的加密算法

**哈希算法（单项加密）**

![image-20211016210209967](E:/typora_img/image-20211016210209967.png)

```
加密过程中不需要使用密钥，输入明文后经过加密算法处理成密文，单项加密得到的密文无法反向解密。
只有重新输入明文，并经过同样的加密算法处理，得到相同的密文并被系统重新识别后，才能真正解密		
算法：MD5/SHA1/SHA224/SHA256/

优点：快速计算m，具有单向性 one-way，不可由散列值推出原消息

场景：文件完整性校验和(Checksum)算法、常规密码等

密码存储常用方式
- 双重MD5
- MD5+加盐
- 双重MD5+加盐
```

- 破解方法：彩虹表暴力破解(https://www.cmd5.com/),

  ​		数据库中存储大量数据，一列明文一列密文，找到对应的密文就能破解明文，没有绝对安全的加密方式，破解只是时间和算力的问题



- 实际中为什么不用更安全的加密算法

  ​		更安全的算法，加密解密更复杂，接口性能下降更严重，在实际生产环境中需要在用户体验和安全性之间做一个权衡

  ![image-20211016214329931](E:/typora_img/image-20211016214329931.png)

**对称加密**

**非对称加密**





## 高并发下账号唯一性保证方案

**简介：高并发下账号唯一性安全保证方案**

* 注册业务

  * 同个时刻注册，需要保证账号在数据库里唯一

* 高并发下问题发现扩大

  * 万分之一的时间，放大100万倍
  * 不是你的代码安全，而是你的并发量过少，几个几十个并发量发现不了问题
  * 几十万几百万并发 ，线下难模拟
    * 代码暂停思维：假如非原子性代码运行到某一行暂停，其他线程重新操作是否会出问题
    * 时间扩大思维：1纳秒的时间，扩大到1分钟，代码逻辑是否会有问题

  

* Redis：先看redis是否有，然后没的话则是新的注册

  * key -value 存储, 配置60秒过期
  * 非原子性操作，存在不一致

* 数据库唯一索引(建表的时间已经添加)

```
 ALTER TABLE user ADD unique(`mail`)
```





## 分布式应用下登录检验解决方案 JWT讲解

**简介：分布式应用的登录检验解决方案 JWT讲解 json web token**

- 什么是JWT

  - JWT 是一个开放标准，它定义了一种用于简洁，自包含的用于通信双方之间以 JSON 对象的形式安全传递信息的方法。 可以使用 HMAC 算法或者是 RSA 的公钥密钥对进行签名
  - **简单来说: 就是通过一定规范来生成token，然后可以通过解密算法逆向解密token，这样就可以获取用户信息**

  ```
        	  {
                  id:888,
                  name:'小D',
                  expire:10000
              }
              
              funtion 加密(object, appsecret){
                  xxxx
                  return base64( token);
              }
  
              function 解密(token ,appsecret){
  
                  xxxx
                  //成功返回true,失败返回false
              }
  ```

  - 优点

    - 生产的token可以包含基本信息，比如id、用户昵称、头像等信息，避免再次查库
    - 存储在客户端，不占用服务端的内存资源

  - 缺点

    - token是经过base64编码，所以可以解码，因此token加密前的对象不应该包含敏感信息，如用户权限，密码等

    - 如果没有服务端存储，则不能做登录失效处理，除非服务端改秘钥

      

- JWT格式组成 头部、负载、签名

  - header+payload+signature
    - 头部：主要是描述签名算法
    - 负载：主要描述是加密对象的信息，如用户的id等，也可以加些规范里面的东西，如iss签发者，exp 过期时间，sub 面向的用户
    - 签名：主要是把前面两部分进行加密，防止别人拿到token进行base解密后篡改token

 

- 关于jwt客户端存储
  - 可以存储在cookie，localstorage和sessionStorage里面



## JWT登录过期-自动刷新token方案

​		在前后端分离场景下，越来越多的项目使用JWT token作为接口的安全机制，但存在token过期后，用户无法直接感知，假如在用户操作页面期间，突然提示登录，则体验很不友好，所以就有了token自动刷新的需求。但是这个自动刷新方案，基本都离不开服务端状态存储，JWT推出思想是：去中心化，无状态化，所以有所违背类似这样的业务，有阿里云首页，没有做token刷新令牌维护，但是符合对应的思想



* 方案一：前端控制检测token，无感知刷新

![image-20211019210511113](E:/typora_img/image-20211019210511113.png)

**基本思想：**
		用户登录成功的时候，一次性返回两个Token，分别为AccessToken和RefreshToken，AccessToken有效期较短,比如1天或者5天，用于正常请求。RefreshToken有效期可以设置长一些，例如10天、20天，RefreshToken存储在Redis中作为刷新AccessToken的凭证
		当AccessToken即将过期的时候，例如提前12小时，客户端请求被拦截器拦截发现距过期时间不足12小时，那么除了正常请求功能接口之外，还应利用请求头中携带的RefreshToken请求token刷新接口获取新的AccessToken，并更新客户端存储中的AccessToken，这样就达到了登录状态保持的效果。

**核心逻辑：**
1、登录成功后，JWT生成AccessToken； UUID生成RefreshToken并存储在服务端Redis中，设置过期时间
2、接口返回3个字段AccessToken/RefreshToken/访问令牌过期时间戳
3、由于RefreshToken存储在服务端Redis中，假如这个RefreshToken也过期，则提示重新登录； 

**疑问：设置RefreshToken和直接延长AccessToken有效期有什么区别？**
		RefreshToken不像AccessToken那样在大多数请求中都被使用，主要是前端检测AccessToken快过期的时候才使用，一般浏览器本地存储的时候，也不叫RefreshToken,，前端可以取个别名，混淆代码让攻击者不能直接识别这个就是刷新令牌。
		直接延长AccessToken有效期，若AccessToken泄露，容易让攻击者一直保持登陆状态，但是对于设置RefreshToken这种方案，假设AccessToken泄露，因为AccessToken有效期较短，在攻击者不知道RefreshToken的情况下，就不会存在一直保活状态的问题，相对来说更加安全。

缺点：前端每次请求都要判断token的过期时间
优点：后端压力小，代码逻辑改动不大



* 方案二：后端存储判断过期时间

```
后端存储AccessToken，每次请求过来都判断是否要过期，如果快要过期则重新生成新的token，并返回给前端重新存储，比如距离1天就过期的情况，如果用户访问对应的接口则会更新，但假如没访问则token已经过期则需要重新登录


优点：前端改动小，只需要存储响应http头里面是否有新的令牌产生，有的话就重新存储
缺点：后端实现复杂，且泄露后容易存在一直保活状态，且前端会存在并发请求，当并发请求收到多个jwt token时，容易生成多个token混乱使用
```



## JWT令牌token泄露恶意使用-解决方案

**讲解：JWT令牌泄露解决方案-避免盗用后被恶意使用**

* 解密：使用互联网大厂的产品时经常遇到这个情况 

  * **比如阿里云或者淘宝，你现在登录了然后换个网络或者地域就需要重新登录**
  * 就是对应的token令牌，不只简单的算法加密，还包括了客户端属性、地理网络位置信息等，一起组成一个token令牌

* 如何避免token令牌泄露被恶意使用

  * ip绑定方式

  ```
  生成token的时候，加密的payload加入当前用户ip。
  
  拦截器解密后，获取payload的ip和当前访问ip判断是否同个，如果不是则提示重新登录
  
  优点：服务端无需存储相关内容，性能高，假如用户广州登录，泄露了token给杭州的黑客，依旧用不了
  
  缺点：如果用户用使用过程中ip变动频繁，则操作会经常提示重新登录，体验不友好
  
  当然也可以让用户开启安全模式和非安全模式，让用户自己知道这个情况，一些区块链、比特币交易所里面就会让用户自己选择控制这个token令牌安全是否和ip、终端、地理网络信息进行绑定
  ```

  

  ```
  					{
                  id:888,
                  name:'小D',
                  ip：“128.23.12.31”
                  expire:10000
              }
              
              funtion 加密(object, appsecret){
                  xxxx
                  return base64( token);
              }
  
              function 解密(token ,appsecret){
  					
                  xxxx
                  //成功返回true,失败返回false
              }
  ```

   









