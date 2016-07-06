---
layout: post
category: note
title: 信息安全原理学习笔记
subtitle: 理论信安
excerpt: 期末复习之信息安全原理  期末信息安全原理复习总结。（由于本月28日也需要去上交面研究生，他们笔试好像就考这门课，所以打算好好总结下）不过这课真的好理论😂
first_time: 2016.06.24 14:50:00
time: 2016.06.24 16:13:09
tags:
- Security
- 信息安全原理

---

# 1. 信息安全概论

## 信息安全的定义
信息安全：防止任何对数据进行未授权访问的措施，或者防止造成信息有意无意泄漏、破坏、丢失等问题的发生，让数据处于远离危险、免于威胁的状态或特性。


- 保密性：对信息资源开放范围的控制。



---

# 2. 信息隐藏

## 定义
信息隐藏（Information Hiding），也叫数据隐藏。简单地说，信息隐藏就是将秘密信息隐藏于另一非保密的载体之中。这里的载体可以是图像、音频、视频、文本 ，也可以是信道，甚至是某套编码体制或整个系统。 






3. 恶意狱警问题: 狱警Willie可能彻底改变通信囚犯的信息，或者伪装成一个囚犯，隐藏伪造的机密信息，发给另外的囚犯。在这种条件下，囚犯可能就会上当，他的真实想法就会暴露无遗。对这种情况，囚犯是无能为力的。不过现实生活中，这种恶意破坏通信内容的行为一般是不允许的，有诱骗嫌疑。目前的研究工作重点是针对主动狱警问题



3. 分类


	![image](http://momomoxiaoxi.com/img/post/shuziyincang.png)

----

# 3. 数字水印
1. 数字水印（Digital Watermark）技术是指用信号处理的方法在数字化的多媒体数据中嵌入隐蔽的标记，这种标记通常是不可见的，只有通过专用的检测器或阅读器才能提取。

3. 主动攻击的目的并不是破解数字水印，而是篡改或破坏水印，使合法用户也不能读取水印信息。

-----

# 4. 消息认证

## 认证的种类

1. 消息认证：消息来源和内容的合法性

## 哈希函数
- 也称散列函数、杂凑函数、数字摘要和数字指纹。
- 它是一种单向密码体制，即它是一个从明文到密文的不可逆映射，即只有加密过程，不能解密。同时，Hash函数可以将任意长度的输入经过变换以后得到固定长度的输出。
-  Hash函数的这种单向特征和输出数据的长度固定的特征使得它可以生成消息或其他数据块的“数据指纹”（也称消息摘要或散列值），因此在数据完整性认证和数字签名等领域有广泛的应用的，哈希函数在现代密码学中起着重要作用。      
-  理论定义：理想的Hash函数是从所有可能的输入值到有限可能的输出值集合的一个随机映射
-  安全性要求
	- 单向性──由消息的散列值倒算出消息在计算上不可行


## 消息认证码
1. 消息认证具有两层含义: 一是检验消息的来源是真实的，即对消息的发送者的身份进行认证; 二是检验消息是完整的，即验证消息在传送或存储过程中未被篡改、删除或插入等。
2. 如果在消息中加入时间及顺序信息，则可以完成对时间和顺序的认证


1. 由来：消息认证无法防止通信双方的相互攻击（欺骗、抵赖等） 

















	
	
	- 认证中心(CA)：


















-----

# 5. 网络安全威胁

## 典型攻击步骤
1. 预攻击检测
2. 发现漏洞，采取攻击行为
3. 获得攻击目标的控制权系统
4. 安装系统后门
5. 继续渗透网络，直至获取机密信息
6. 消灭踪迹

## IP欺骗
1. IP欺骗：IP欺骗指攻击者假冒他人IP地址,发送数据包
2. 因为IP协议不对数据包中的IP地址进行认证, 因此任何人不经授权就可伪造IP包的源址。

## ARP攻击
ARP攻击就是通过伪造IP地址和MAC地址实现ARP欺骗，能够在网络中产生大量的ARP通信量使网络阻塞，攻击者只要持续不断的发出伪造的ARP响应包就能更改目标主机ARP缓存中的IP-MAC条目，造成网络中断或中间人攻击。

## ICMP攻击

- ICMP利用攻击：由于主机对ICMP数据报不作认证，使得攻击者可以伪造ICMP包使源主机产生 错误的动作从而达到特定的攻击效果。

- ICMP主机不可达和TTL超时报文：此类报文一般由IP数据报传输路径中的路由器发现错误时 发送给源主机的，主机接收到此类报文后会重新建立TCP 连接。攻击者可以利用此类报文干扰正常的通信。

- ICMP重定向报文：TCP/IP体系不提供认证功能，攻击者可以冒充初始网关向 目标主机发送ICMP重定向报文， 诱使目标主机更改寻径 表，其结果是到达某一IP子网的报文全部丢失或都经过一 个攻击者能控制的网关。

## 电子邮件欺骗
1. 攻击者伪装为系统管理员给用户发送邮件要求用户修改口令或者伪造一个友好的发件人地址,在貌似正常的附件中加载病毒或其他木马程序。

2. 对于伪造电子邮件地址防范方法是使用加密的签名技术，像PGP来验证邮件，通过验证可以保护到信息是从正确的地方发来的，而且在传送过程中不被修改。

## DNS域名欺骗
参照长城防火墙原理即可（即建立一个假的的DNS服务器，给予假的DNS信息）

## ping of Death

1. 攻击者：发送长度超过65535的ICMP Echo Request的碎片分组
2. 目标机：重组分片时会造成事先分配的65535 字节缓冲区溢出，导致TCP/IP堆栈崩溃

## Unicode输入验证攻击
当微软IIS 4.0／5.0（远东地区版本）在处理包含有不完整的双字节编码字符的HTTP命令请求时，会导致 Web目录下的文件内容被泄漏给远程攻击者。

	“%c1%hh”→”0xc10xhh”→”/” 
	“%c0%hh”→”0xc00xhh”→”\”


## sql注入
SQL注入是一种利用用户输入构造SQL语句的攻击。

## 漏洞扫描基本方法
漏洞扫描的基本方法有两类:

1. 漏洞特征库匹配 
2. 模拟攻击

## 拒绝服务攻击
1. Ping flooding的原理是:在某一时刻,多台主机对目标主机使用Ping程序,以耗尽 目标主机的网络带宽和处理能力,达到攻击目标的目的。
2. SYN Flood 原理：TCP连接的三次握手和半开连接 攻击者：发送大量伪造的TCP连接请求 方法1：伪装成当时不在线的IP地址发动攻击 方法2：在主机或路由器上阻截目标机的SYN/ACK分组 目标机：堆栈溢出崩溃或无法处理正常请求
3. 电子邮件炸弹。指的是用伪造的IP地址或电子邮件地址向同一信箱发送数以千计的内容相同或者不同的垃圾邮件，致使受害人邮箱的容量无法承受这些邮件，严重的可能会导致电子邮 件服务器操作系统瘫痪。

## 分布式拒绝服务攻击


### 定义
分布式拒绝服务（Distributed Denial of Service，DDoS）攻击 指借助于客户/服务器技术， 将多个计算机联合起来作为攻击平台，对一个或多个目标发动DoS攻击，从而成倍地提高拒绝服务攻击的威力。

### 攻击准备
1)攻击者攻击诸客户主机以求分析他们的安全水平和脆弱性。 

2)攻击者进入其已经发现的最弱的客户主机之内(“肉机”)，并且秘密地安置一个其可远程控制的代理程序(端口监督程序demon)。

### 发起攻击
3)攻击者使他的全部代理程序同时发送由残缺的数字包构成的连接请求送至目标系统。 

4)包括虚假的连接请求在内的大量残缺的数字包攻击目标系统，最终将导致它因通信淤塞而崩溃。

### 检测方法	
1. 根据异常情况分析
	
	网络的通讯量突然急剧增长，超过平常的极限值时； 网站的某一特定服务总是失败； 发现有特大型的ICMP和UDP数据包通过或数据包内容可疑。

2. 使用DDOS检测工具
	
	扫描系统漏洞是攻击者最常进行的攻击准备

### 防御策略
1. 及早发现系统存在的攻击漏洞，及时安装系统补丁程序。
2. 经常检查系统的物理环境，禁止那些不必要的网络服务。建立边界安全界限，确保输出的包受到正确限制。经常检测系统配置信息，并注意查看每天的安全日志。

## 病毒
1. 计算机病毒,是指编制或者在计算机程序中插入的破坏计算机功能或者毁坏数据，影响计 算机使用，并能够自我复制的一组计算机指令或者程序代码
2. 特征：传染性 非授权性 隐蔽性 潜伏性 破坏性 不可预见性

## 木马
木马（ Trojan horse ）是一种基于远程控制的黑客 工具，具有：隐蔽性、潜伏性、危害性、非授权性。

木马与病毒的区别
	
	1. 病毒程序是以自发性的破坏为目的 
	2. 木马程序是依照黑客的命令来运作，主要目的是偷取文件、机密数据、个人隐私等行为。 
	3. 隐蔽、非授权性

## 蠕虫
计算机蠕虫是指一种利用安全漏洞，在网络中主动传 播，并造成信息泄漏、系统破坏或拒绝服务等后果的 有害程序。
![image](http://momomoxiaoxi.com/img/post/ruchongyubingdu.png)


-------

# 6.网络访问控制

## 防火墙

### 定义

是一种高级访问控制设备，置于不同网络安全域之间的一系列部件的组合，它是不同网络安全域之间通信流的唯一通道，能根据有关的安全策略控制（允许、拒绝、监视、记录）进出网络的访问行为。（可在链路层、网络层、应用层实现）

### 功能

其功能的本质特征是隔离内外网络和对进出信息流实施访问控制。隔离方法可以是基于物理的，也可以是基于逻辑的；











	1、防火墙能强化安全策略。
	2、防火墙能有效地记录Internet上的活动，作为访问的唯一点，防火墙能在被保护的网络和外部网络之间进行记录。
	3、防火墙限制暴露用户点，能够防止影响一个网段的问题通过整个网络传播。
	4、防火墙是一个安全策略的检查站，使可疑的访问被拒绝于门外。
	二、缺点：
	1、防火墙可以阻断攻击，但不能消灭攻击源。
	2、防火墙不能抵抗最新的未设置策略的攻击漏洞。
	3、防火墙的并发连接数限制容易导致拥塞或者溢出。
	4、防火墙对服务器合法开放的端口的攻击大多无法阻止。
	5、防火墙对待内部主动发起连接的攻击一般无法阻止。
	6、防火墙本身也会出现问题和受到攻击，依然有着漏洞和Bug。
	7、防火墙不处理病毒。



------

# 7. 入侵检测

## 定义

### 入侵

入侵是指未经授权蓄意尝试访问信息、窜改信息，使系统不可靠或不能使用的行为。它企图破坏计算机资源的：1.完整性(Integrity)2.机密性（Confidentiality）3.可用性（Availability）4.可控性（Controliability）



### 定义


1. 监控、分析用户和系统活动
















## 蜜罐技术
Honeypot是一种资源，它的价值是被攻击或攻陷。 

-----

# 8. 网络通信安全

## VPN

### 定义

虚拟专用网是在公共网络中建立的安全网络连接，这个网络连接和普通意义上的网络连接不同之处在于，它采用了专有的隧道协议，实现了数据的加密和完整性检验、用户的身份认证，从而保证了信息在传输中不被偷看、篡改、复制，从网络连接的安全性角度来看，就类似于在公共网络中建立了一个专线网络一样，只不过这个专线网络是逻辑上的而不是物理的，所以称为虚拟专用网。 



4. 安全关联－SA








------

# 9. 数据库

## 定义
数据库(Database,简称DB)是长期储存在计算机内、有组织的、可共享的大量数据的集合。














------

# 10. 数据库的恢复

## 事务的定义



## 事务（ACID）特性

1. 原子性（Atomicity）


1. 有的是可以通过事务程序本身发现的、



#### 恢复措施
撤消事务（UNDO

#### 定义


#### 定义





-----

# 11. 无线网络的安全

## 蓝牙技术

蓝牙是一种近距离无线通信技术规范，用来描述各种电子产品相互之间如何用这种无线协议进行连接。

## Zigbee

ZigBee技术是一种近距离、低复杂度、低功耗、低速率、低成本的双向无线通讯技术。早期也被称为 “HomeRF Lite”、“RF- EasyLink”或“fireFly”无线电技术，目前统称为ZigBee技术

## 红外技术

红外线通讯是一种廉价、近距离、无线、方向性强、保密性强的通讯方法。
第十一章 无线网络的安全 只有小题

### SSID

#### 1. 开放认证

设计目的是允许设备快速接入网络。开放认证由认证请求和认证响应两个消息组成，允许任何设备接入。如果没有加密机制，任何知道无线接入点SSID的设备都可以接入网络。如果使用WEP加密，则WEP密钥本身是一种接入控制。如果一个设备没有正确的WEP密钥，即使认证通过，设备也不能通过无线接入点传送数据。




攻击者仅需要将无线网卡设置在混杂模式，同时安装一个捕获数据报文的工具软件(如sniffle)，便可以捕获到网络流量并对其进行分析，通过流量分析，攻击者可以获得4类信息：








1. 制定无线安全策略
2.  激活WEP
3.  创建访问控制列表
4.  使用动态密钥交换机制
5.  保持固件的更新
6.  访问点的适当安装
7.  给访问点设置强口令
8.  改变SSID
9.  禁止广播SSID（一般只需要配置这个）
10.  限制无线电波的传播范围
11.  布置访问控制器
12.  实现个人防火墙
13.  使用虚拟专用网
14.  使用静态IP地址
15.  控制WLAN的使用
16.  所有的无线站都是不可信的
17.  监视非授权访问点

###  参考：
1. PPT