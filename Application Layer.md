## Application Layer

- 向网络应用提供通用的服务
  - 端对端
- Telnet 远程登录
- SMTP 电子邮件
- FTP 文件传输
- HTTP

#### DNS 域名系统

![image-20210105232137326](Application%20Layer.assets/image-20210105232137326.png)

- 每个层都有共性的问题：寻址

- 路由器认识的是IP地址，域名给人看

- 两者之间需要桥梁

- **域名空间**

  - www.zju.edu.cn
  - cn：中国，edu：教育科研网，zju：浙大，www：web
  - cn：顶级域名
    - 通用的顶级域名 / 与国家地区有关的顶级域名
    - com：通用的顶级域名，一般表明是公司
    - org：非盈利性组织
    - net：互联网信息中心
    - edu：美国的高等学校
    - gov：美国的联邦政府
    - int：国际组织
  - 二级域名
  - 绝对域名 / 相对域名
    - 绝对域名需要加上根：www.zju.edu.cn.
    - 字符不区分大小写
  - 有可能多个域名对应一个IP地址

- **域名数据库**

  - IN：表示是互联网用的
    - DNS一开始不是只为互联网设计
  - 域名服务器中的cache是有时限的
    - 86400为初始值，会不断减一
    - 如果写0，就不需要cache，这样就可以保证每次都是最新的
  - 下图不是cache，是我自己知道的东西
    - **这个表每年都考**

  <img src="Application%20Layer.assets/image-20201229102711396.png" alt="image-20201229102711396" style="zoom:50%;" />

  - 字段类型

    - IPv6的地址长度：128位，对应四个A

    - MX：专门给电子邮件系统用的

      ![image-20201229113755636](Application%20Layer.assets/image-20201229113755636.png)

      - 10：33 没听明白
    - A
    
    <img src="Application%20Layer.assets/image-20201229102823799.png" alt="image-20201229102823799" style="zoom:50%;" />

- **域名服务器**

  <img src="Application%20Layer.assets/image-20201229104302585.png" alt="image-20201229104302585" style="zoom:50%;" />

  - 怎么通过域名找IP地址

  - 每个圈代表一个区域的权威域名服务器（是人直接做到数据库里的）

  - 服务器之间也可以认识

  - 怎么问别人

    - 递归方式 recursive query

      - 我去问计算机学院DNS，计算机学院也不认识，于是就问浙大的域名服务器，浙大也不知道，于是问cernet(China Education and Research Network)，于是找到清华的域名服务器

    - 迭代方式 iterative query

      - 不知道，告知我不知道，然后把更高级的地址给我

    - 目前一般用两种方式的组合

      <img src="Application%20Layer.assets/image-20201229105033473.png" alt="image-20201229105033473" style="zoom: 67%;" />

      - 最低层用递归的方式，DNS与DNS之间一般用重定向

#### Electronic Mail

- SMTP协议：简单邮件传送协议

  - TCP连接

- <img src="Application%20Layer.assets/image-20201229105140253.png" alt="image-20201229105140253" style="zoom: 67%;" />

  - TCP和UDP：要求两端都在线
  - DTN：延迟容忍的网络 Delay Tolerant Networks
  - 因此需要另外一个地方：邮箱
    - 会做一个目录（文件夹）
    - MTA
    - 接收方接收：不使用SMTP，用POP3或IMAP（最后一级的投递）
    - 发送方用什么？SMTP

- The User Agent

- Message Formats

  - 偏移量不确定，文本方式
  - 但应用层不一定全是文本方式，也有二进制方式（有一种媒体）
  - 原本的SMTP要求是可打印字符
  - 怎么解决非字符的传输
    - 让正文部分支持不是可打印的字符
    - 想办法让UA提供手段转换不是可打印的字符
  - base64（考试也喜欢考）
    - 见补充课件
    - 编出来几个字符（注意回车换行）
  - 正文部分又套了个MIME协议

- Final Delivery

  - POP3：针对个人用户
    - 缺省的话，邮件被拿走，邮箱里不会保存
  - IMAP：针对商务的比较多
    - 缺省的话，是会保存的

  <img src="Application%20Layer.assets/image-20201229115749930.png" alt="image-20201229115749930" style="zoom: 50%;" />

#### The World Wide Web

- B/S
  
- 三层cs
  
- url：通用资源定位服务，解决网页的寻址问题

  - 不区分大小写
  - 三个部分
    - 用什么协议
    - 结点的地址：可以是IP地址或域名
    - 指定一个网页

  <img src="Application%20Layer.assets/image-20210105095635063.png" alt="image-20210105095635063" style="zoom:50%;" />

- 浏览器 + 插件

- TCP面向连接，三次握手连接建立，数据通信，两个半连接的文明释放

  - 连接维持成本高：至少七个包

  - 现在互联网的背景：很多都是短连接

  - HTML：超文本标记语言

  - HTTP：浏览器和服务器之间通信的协议

  - 针对短连接：拥塞控制、流量控制，优化连接建立的优化

  - 建立连接池，维持稳定的长连接，降低负载

    - 可能也有进程池，帮助建立dynamic网页

  - 中间件

    <img src="Application%20Layer.assets/image-20210105100836377.png" alt="image-20210105100836377" style="zoom:50%;" />

- 不登陆可以直接访问网页（cookie的例子）

  <img src="Application%20Layer.assets/image-20210105101720736.png" alt="image-20210105101720736" style="zoom: 67%;" />

  - 在reply网页的时候，捎上cookie，浏览器拿到后会存下来，以后再访问相同网站时，网站会问有没有cookie，送回后就知道以前访问过（知道是谁）

- 网页

  <img src="Application%20Layer.assets/image-20210105102035267.png" alt="image-20210105102035267" style="zoom: 67%;" />

  - HTML做的，描述了网页要显示的内容，怎么显示

  - 用户用表的方式提交给服务器

  - 事先做好的网页：static

  - 查了数据库的数据再做出来的网页：dynamic

    - 下面两种都是动态
    - 另一种说法：static / dynamic / active
    - 服务器侧的动态(dynamic) / 客户侧的动态(active)

    <img src="Application%20Layer.assets/image-20210105102850434.png" alt="image-20210105102850434" style="zoom: 67%;" />

  - 外汇行情不停变化：不点也可以刷新？

    - 动态网页不够：只是在服务器是活的，浏览器拿到还是死的
    - 网页的程序：Java, JavaScript，把程序在浏览器上运行，程序可以不断查询DB

  - 网页是用HTML写的，显示什么和怎么显示

    - 与HTML相应的：XML

- HTTP：超文本传送协议

  - 一个服务器进程监听TCP的80端口

  - HTTP协议是无状态的
  
  - 因此有了cookie：存储在用户主机的文本文件
  
- 类型，叫做Method
  
  - GET：根据url取得网页
    - ...
  
    <img src="Application%20Layer.assets/image-20210105104201509.png" alt="image-20210105104201509" style="zoom:50%;" />
  
  - The status code response groups
  
    <img src="Application%20Layer.assets/image-20210105104414999.png" alt="image-20210105104414999" style="zoom:67%;" />

#### Streaming Audio and Video

- 流媒体
- 大的声音对应比特少，小的声音对应比特多
  - 做对数
- PCM：采样，量化，编码 
- ADPCM：自适应的PCM，传输差值
- 共振峰方式：压缩比可以做到很大



## Network Security

- 四个方面的问题
  
  - 加密
  - 反拒认：数字签名
  - 防篡改：数字摘要
  - 认证：证书
  
- 密码学，密码分析学

  - 对称密码体制：DES, AES
    - 加密和解密用同一密钥
  - 非对称密码体制：RSA
    - 加密和解密密钥不同：公钥加密，解密私钥

- 数字签名

  - 假定以下是A的公钥及私钥：

    n=F03E1D9F9BFB6827B4A49AED686F7790868CA58FA7BA7110C29D3241A8E2EF53

    p=F9298817691C971281C3CD6D203C28BF

    q=F6D5EB95C1915957FB00604580B9EA6D

    d=08F1C718922E220A9867287D7E4DE81D9EED52D623E1BB48758146F22C515D41

    e=010001

     

    假定A要发一封信给B:

    信的内容L="Hello, I'm A.ABCD"

    如何对信进行加密？

    L' = RSA(L, B的公钥)

    A把L'发给B，B收到后如何解密？

    L = RSA(L', B的私钥)

     

    A如何对信件进行签名?

    首先对信的内容计算摘要(digest),这里采用MD5算法:

    M = MD5(L) = 82228599d3e30286a22e7d170ebe2546

    用A的私钥对M进行签名(实际上是用A的私钥对M加密):

    M' = RSA(M, A的私钥) =

    6EFADDDBBEC3F995F4F4398F2A6049BF42A41645B36B1A2E1AE6C52BFE11950B

    此时，M'就是A对信件摘要M的签名。

    假定A把L及M'都发送给B。

    B如何对A的签名M'进行验证？

    首先要用A的公钥对M'进行解密：

    m = RSA(M', A的公钥) = 82228599d3e30286a22e7d170ebe2546

    最后还要判断m是否正确:

    若MD5(L) = m，则证明此信确实是A所发。

- 防篡改

  - MD5
  - SHA-3

- 证书

  - CA

- IPsec

- 防火墙

  - 公网和内网之间

- VPN

  - virtual private network

- PGP

  - 考过很多次

  <img src="Application%20Layer.assets/image-20210112114743674.png" alt="image-20210112114743674" style="zoom:67%;" />

- SSL



- 

