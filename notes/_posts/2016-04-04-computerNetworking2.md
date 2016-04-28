---- 
layout: post
category: note
title: 计算机网络学习笔记2
subtitle: 计算机网络运输层学习
excerpt: 最近，一直忙着SIT的收尾工作，另外还得忙一个新项目－敏感信息收集相关的。因此，就一直没有时间静下来写点东西。正好，这周忙的差不多了。偷闲写些笔记。本次打算梳理下运输层相关的东西，主要是TCP与UDP的实现原理与具体细节分析———————正好一个科研项目也需要这一块的知识，fighting！：）
first\_time: 2016.04.27 15:31:00
time: 2016.04.28 16:13:09
tags:
- 计算机网络
- 运输层
- TCP／UDP

---- 

# 计算机网络运输层学习总结
运输层居于网络层之上，应用层之下。它主要为运行在**不同主机上的应用进程**之间提供逻辑通信。与之相对的网络层协议是为了**两台不同的主机之间**提供逻辑通信。
运输层主要有TCP与UDP协议，是在端系统中而不是在网络路由器中实现的。
主要有以下几个知识点：
1. 多路复用和多路分解
 2. UDP协议
 3. TCP协议
 4. TCP协议的可靠传输机制
 5. TCP协议的流量控制
 6. TCP协议的拥塞控制
 7. TCP协议的连接管理

---- 
## 1. 多路复用和多路分解
> 多路分解是将在运输层收到的报文段中的数据交付到正确的套接字的工作。
> 多路复用是从源主机的不同套解字中收集数据块，并为每个数据块封装上首部信息（这将在多路分解时使用）从而生成报文段，然后将报文段传递到网络层的工作。
- 复用：发送方不同的应用进程都可以使用同一个运输层协议传输数据
- 分用：接收方的运输层在剥去报文的首部后能把这些数据正确交付目的应用进程
### 1.2无连接的多路复用和多路分解
UDP是无连接的。一个UDP套接字是由一个包含目的IP地址和目的端口号的二元组来全面标识的。  
如果两个UDP报文段具有不同的源IP/端口号，但具有相同的目的IP/端口号，那么，这两报文段会被送至相同的目的进程。
所以，一个UDP的套接字使用的就是本地的IP和端口号进行标识。这样运输层中收到UDP报文段时，就会根据报文段中的目的IP/端口号向进程投递报文。
### 1.3面向连接的多路复用和多路分解
TCP是面向连接。一个TCP套接字是由一个四元组（源IP/端口号，目的IP/端口号）来标识的。  
这样当一个TCP报文段从网络到达一台主机时，主机使用全部四个值来将报文段定向（多路分解）到相应的套接字。
正是因为TCP是面向连接的，所以需要四个值来标识。不同客户机里面的不同进程都可以跟服务器的一个特定的服务进程建立各自的一条连接。而这每一条连接的两端的套接字都是以四个值进行标识的。  
服务器关注连接请求报文段里的这四个值，新创建的连接套接字通过四个值来标识。所有后续到达的报文段，如果它们这四个值都匹配，则被多路分解到这个套接字。
---- 
## 2.UDP协议
1. UDP协议仅在网络层协议的基础上增加了一点多路复用和多路分解服务以及差错检测功能。
2. UDP的特点：
	- 无连接，尽最大努力交付，面向报文，没有拥塞控制，支持一对一，一对多，多对一和多对多的交互通信，首部开销小
3. UDP报文结构
	报头：
	![][image-1]
	ipv4结构：
	![][image-2]
	![][image-3]
	注意，图中的蓝色部分的两个源端口是错误的。正确的是先从左往右：源端口、目的端口。  
	UDP报文首部真正只具有四个字段，每个字段只有2个字节，所以一共是8个字节。黄色部分的伪首部是用于求检验和字段的。  
	在计算检验和的时，在UDP用户数据报前增加12个字节的伪首部。伪首部既不向下传送也不向上递交，仅仅是为了计算检验和。  
	UDP的差错检测是包括数据部分，与IP只检测首部是不同的。
	UDP的检验和求法采用的是IP的16位的反码求和。若UDP用户数据报的数据部分不是偶数个字节则填入一个全零字节（这个字节不发送），在接收方，对收到UDP数据报进行16位的反码求和，得到结果如果为全1则无差错。虽然UDP提供差错检测，但不提供差错恢复。
	![][image-4]
4. UDP的注意点：
	1. UDP的检验和是可选的。
	2. UDP的加伪首部去计算检验和的目的确认数据是否正确到达目的地（IP没有接受地址不是本主机的数据和IP没有把应传送给另一高层的数据报传给UDP）。
	3. UDP的检验和计算出来全是0的话，就往检验和字段填充全1，这是在二进制反码计算中是等效的。但如果该字段全0则说明发送端没有做计算。
	4. 如果UDP数据报有错，会被悄悄地丢弃。
	虽然UDP是无连接，不可靠的协议。但是其占用资源小，速度快，实时性强等优点还是受到很多应用利用。
---- 
### 3.TCP协议
**TCP协议是面向连接，提供可靠交付服务，提供全双工通信，面向字节流的。**
面向连接是指通信前会在两个主机两个不同的通信进程之间建立一条虚拟的连接（逻辑连接）。
TCP报文结构：
![][image-5]
1. 源端口或目的端口（分别2个字节）：略。
2. 序号（4个字节）：TCP连接中传送的字节流中的每一个字节都按顺序编号。传送的字节流的起始序号必须在连接建立时设置。首部中的序号字段值的是本报文段所发送的数据的第一个字节的序号。例如一报文段序号值为101，数据共有100字节，那么下一报文段序号值就为201。因为序号的大小是[0,2^32 -1](),超出了循环从0开始，所以序号是使用mod2^32运算的。
3. 确认号（4个字节）：期望收到对方下一个报文段的第一个数据字节的序号。
4. 数据偏移（4位）：指出TCP报文段的数据起始处距离TCP首部的起始处有多远。实际上就是指出TCP首部长度。数据偏移的单位是4个字节，而4位二进制的最大值是15，则说明首部最大只能是15＊4=60字节。其实选项字段最大只能为40字节。
5. 保留（6位）：略。
6. 紧急URG：表明紧急指针子字段有效。
7. 确认ACK：等于1时确认号字段才有效。TCP规定连接建立后所有传送报文度必须把ACK置1。
8. 推送PUSH：表明需要尽快地交付接收应用进程，不再等整个缓存都填满后再向上交付。
9. 复位RST：等于1时表明TCP连接中出现严重差错，必须释放连接再重新建立。
10. 同步SYN：用于连接建立时同步序号的。
11. 终止FIN：用来释放一个连接。
12. 窗口（2个字节）：该字段用于流量控制，指示接收方愿意接收的字节数量。因为接收方的接收缓存有限。窗口值是动态变化的。
13. 检验和（2个字节）：检验和检查的范围包括首部和数据。
14. 紧急指针（2个字节）：指出紧急数据的末尾在报文段中的位置。即使窗口值为零也可以发送紧急数据。
15. 选项（最大可达40个字节）：略。

---- 
### 3.TCP协议的可靠传输机制
TCP协议的可靠传输机制主要依靠：
1. 报文确认
	2. 超时重传
	3. 快速重传
	4. 差错恢复
#### 报文确认
TCP协议规定，接收方对于正确接收到的来自发送方的报文段要给予确认返回报文。该报文中的首部ACK指向接收方期待下一个开始接收的字节。
#### 超时重传
当发送方发送报文之后，会启动一个倒数计时器（重传超时间隔），计时器为零时，就认为报文可能在网络中丢失，需要重传报文。
**那么这个重传超时间隔的值要设为多少呢？**
首先，这个值必须大于TCP连接的往返时延（RTT）。  
TCP采用的是自适应的方法。设SampleRTT为样本RTT，就是从某报文被发出（即交给IP）到对该报文段的确认被收到之间的时间量。  
在任意时刻只为一个已发送但未被确认的报文段估测量，并且仅为传输一次的报文段测量SampleRTT。
可是SampleRTT的值会因为网络的拥塞情况而不段的变化，所以需要一个均值EstimatedRTT：
SampleRTT = (1-a)＊EstimatedRTT + a＊SampleRTTs
RFC 2988中给出a参考值是0.125。从统计学观点来说，这种平均被称为指数加权移动平均。
另外，测量RTT的变化也是有意义的。所以RFC 2988定义了RTT偏差DevRTT。
DevRTT = (1-b) * DevRTT + b * |SampleRTT - EstimatedRTT|
超时间隔应该是等于大于均值的，而超过的均值的范围值可以利用DevRTT。所以超时间间隔的公式就为
TimeoutInterval = EstimatedRTT + 4 * DevRTT
#### 快速重传
超时触发重传存在的另一个问题就是超时周期可能相对较长。这种长超时周期迫使发送方等待很长时间才重传丢失分组。幸运的是，发送方可以通过冗余ACK较好地检测丢包情况。
冗余ACK就是再次确认某个报文段的ACK，而发送方先前已经收到对该报文段的确认。







---- 
# 参考：
1. [\[http://www.zhuangjingyang.com/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C%E5%BA%94%E7%94%A8%E5%B1%82%E7%9F%A5%E8%AF%86%E6%80%BB%E7%BB%93.html]][2]
2. [http://zhenhua-lee.github.io/tech/network.html][3]
3. [http://blog.163.com/magicc\_love/blog/static/][4]

[2]:	https://zh.wikipedia.org/wiki/%E7%94%A8%E6%88%B7%E6%95%B0%E6%8D%AE%E6%8A%A5%E5%8D%8F%E8%AE%AE
[3]:	http://biousco.github.io/2015/07/11/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C-%E8%BF%90%E8%BE%93%E5%B1%82/
[4]:	http://cbsheng.github.io/2014/12/10/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C%E7%AC%94%E8%AE%B0-%E8%BF%90%E8%BE%93%E5%B1%82/

[image-1]:	momomoxiaoxi.com/img/network/1.png
[image-2]:	momomoxiaoxi.com/img/2.png
[image-3]:	momomoxiaoxi.com/img/3.jpg
[image-4]:	momomoxiaoxi.com/img/4.jpeg
[image-5]:	momomoxiaoxi.com/img/network/5.jpeg