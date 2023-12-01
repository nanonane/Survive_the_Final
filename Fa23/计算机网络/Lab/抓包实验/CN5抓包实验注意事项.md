# CN5抓包实验注意事项

@O2iginal 本文档主要针对课程教材CN5抓包实验过程中的容易忽视的地方进行说明，并对一些过时的文档内容进行了更新。祝大家实验顺利！

## lab1-http

### 01 关于Telnet的使用
CN5实验文档中的telnet命令使用方法，实际上是linux下的telnet操作方法，而在windows下略有不同。

#### 1.1 linux下的telnet命令使用方法
```shell
# 1. 命令行输入telnet命令
telnet www.baidu.com 80

# 2. 输入http请求报文
GET / HTTP/1.1
Host: www.baidu.com

# 3. 按两次回车键，发送请求报文
# 此时应得到响应报文
```

#### 1.2 windows下的telnet命令使用方法：
```shell
# 1. 命令行输入telnet命令（笔者使用Windows Terminal终端）
telnet www.baidu.com 80

# 2. 按下Ctrl+]，进入telnet命令行模式，应有如下提示
# Welcome to Microsoft Telnet Client
# Escape Character is 'CTRL+]'
# Microsoft Telnet>

# 3. 回车，上述3行提示消失，此时可以输入http请求报文
GET / HTTP/1.1
Host: www.baidu.com

# 4. 按两次回车键，发送请求报文
# 此时应得到响应报文
```

#### 1.3 其他建议
1. 建议使用linux系统进行实验，有如下两点原因：
    - linux系统下的telnet命令使用方法与实验文档中的命令使用方法一致，且更加简洁；
    - 在笔者的机器上，Windows下的返回报文排版混乱，不易于阅读；而linux下的返回报文排版清晰；
2. 对于使用linux系统，建议安装WSL2：
    - 可参考笔者的配置记录 https://www.cnblogs.com/o2iginal/p/17755371.html
    - 对于本课程实验的很多命令行工具，会发现在Linux中使用更加方便，且配置更加简单（linux可以直接使用apt-get安装，而windows需要下载安装包，配置环境变量等）；

### 02 关于本次http实验使用的url

因为实验文档为2012年编写，但此时大多数网站已经使用https协议，不符合实验要求。在此提供笔者使用的url，供大家参考。

```shell
# 1. 图片url
http://www.baidu.com/img/flexible/logo/plus_logo_web_2.png

# 2. 网页url
http://www5.baidu.com/
```
但要注意，可能会发现，在你的浏览器中，这些url都会自动跳转到https协议，这是因为浏览器会自动将http协议的url转换为https协议的url。因此，需要在浏览器中进行相关设置，关闭自动跳转。

关闭自动跳转可参考：https://zhuanlan.zhihu.com/p/589687611

### 03 关于本次http实验Wireshark的使用

实验文档中Wireshark的过滤器规则已经过时，笔者提供如下过滤器规则，供大家参考。

```shell
tcp.port==80
```

### 04 其他注意事项

在Step4中，需要探索`Last-Modified`字段的效果。但可能在实验中可能会出现如下情况：在正式抓包前已经访问过该网页，导致`Last-Modified`字段的值为访问该网页的时间，而正式第一次抓包时已经存在缓存，因此无法观察到`Last-Modified`字段的效果。

建议在抓包前，清除浏览器缓存，或者使用无痕模式进行抓包。

## lab2-dns

### 01 如何获得根服务器的IP地址

有如下两种方法：
1. 直接在搜索引擎中搜索`根域名服务器IP地址`……
2. 使用`nslookup`命令查询根域名服务器的IP地址，命令如下：
```shell
nslookup -type=ns .
```

### 02 关于dig的安装

如上个实验的建议，建议使用linux系统进行实验，因linux下dig安装更加方便。

而在Windows下，dig的安装需要下载安装包，配置环境变量等。需下载[BIND](https://www.isc.org/bind/)，其中包含dig工具。

### 03 如何使用Wireshark的IO图

在Step5中，需要查看IO图反映了DNS响应时间的分布情况。但当前IO Graph的使用界面变换较大，笔者提供如下使用方法，供大家参考。

#### 3.1 坐标轴缩放

新版的Wireshark中，坐标轴缩放的操作需要通过鼠标右键菜单进行，具体操作如下：

1. 在Wireshark中，点击`Statistics`菜单，选择`IO Graphs`，进入IO Graphs界面；
2. 鼠标右键，弹出如下菜单，通过这里的选项对X、Y轴进行设置；
![Alt text](CN5%E6%8A%93%E5%8C%85%E5%AE%9E%E9%AA%8C%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9.assets/image.png)

#### 3.2 `dns.time`选项设置

设置选项位置如下图所示：
![Alt text](CN5%E6%8A%93%E5%8C%85%E5%AE%9E%E9%AA%8C%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9.assets/image-1.png)

## lab3-tcp

### 01 关于本次tcp实验使用的url

在Step5中，需要观察传输速度稳定后的情况。因而建议选择较大的文件进行下载，以便能够得到稳定的传输速度。

建议事先测试`wget`所需时间，**至少持续若干秒**才能得到稳定的传输速度。

### 02 关于Wireshark过滤器设置

本实验的过滤器设置因选择的url不同而不同，笔者的过滤器设置如下：

```shell
tcp and ip.host==server.hosting3.acm.org
```

对于上面的`ip.host`，若不清楚，可先进行一次抓包，观察Wireshark得到报文，如下图所示：
（在打开域名解析的情况下，Wireshark会自动将域名解析为IP地址）
![Alt text](CN5%E6%8A%93%E5%8C%85%E5%AE%9E%E9%AA%8C%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9.assets/image-2.png)

### 03 关于Wireshark IO图的使用

关于IO图的缩放，已在lab2中进行了说明，基本上是通过鼠标右键菜单进行设置。

实验时需要对IO图中的流量进行过滤，选项位置如下图所示：
![Alt text](CN5%E6%8A%93%E5%8C%85%E5%AE%9E%E9%AA%8C%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9.assets/image-3.png)

但在笔者实验过程中，发现IO图中的流量过滤选项或许无效（可能仍是操作不正确），因而建议直接在Wireshark主界面中进行过滤，设置好过滤器后，再进入IO图界面，即可得到过滤后的IO图，如下图所示：

![Alt text](CN5%E6%8A%93%E5%8C%85%E5%AE%9E%E9%AA%8C%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9.assets/image-4.png)

此外，应注意IO图的y轴单位，可能需要进行转换，如下图所示：

![Alt text](CN5%E6%8A%93%E5%8C%85%E5%AE%9E%E9%AA%8C%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9.assets/image-5.png)

## lab4-udp

本实验无特殊注意事项。

这里仅说明`promiscuous mode`、`name resolution`的设置方法，如下图所示：

![Alt text](CN5%E6%8A%93%E5%8C%85%E5%AE%9E%E9%AA%8C%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9.assets/image-6.png)

![Alt text](CN5%E6%8A%93%E5%8C%85%E5%AE%9E%E9%AA%8C%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9.assets/image-7.png)

## lab5-ipv4

### 01 关于本次ipv4实验使用的url

注意，本实验需要ipv4协议，而非ipv6协议。Step5中涉及到计算校验和，而ipv6头部中没有校验和字段，因而必须在实验前对所选url进行测试，以确保使用的是ipv4协议。

例如，检查`www.baidu.com`，使用`wget`抓取网页，同时Wireshark抓包，观察报文，发现是ipv6协议，如下图所示：

![Alt text](CN5%E6%8A%93%E5%8C%85%E5%AE%9E%E9%AA%8C%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9.assets/image-8.png)

而对于`www5.baidu.com`，使用`wget`抓取网页，同时Wireshark抓包，观察报文，发现是ipv4协议，如下图所示：

![Alt text](CN5%E6%8A%93%E5%8C%85%E5%AE%9E%E9%AA%8C%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9.assets/image-9.png)

当然，实际上`wget`有可选参数，可以指定使用的协议，但这里不再赘述，可自行查阅。

笔者在本实验使用`http://csapp.cs.cmu.edu/`（服务器在国外，因而路径较长，且在tracert过程中可出现若干超时结点）。

### 02 Wireshark过滤器设置

设置过滤器如下：
```shell
tcp.port==80
```

## lab6-icmp

### 01 关于本次icmp实验使用的url

笔者所用url如下：
```shell
www5.baidu.com
```

### 02 Step4问题2的建议

可设置Wireshark过滤器，方便统计报文个数，如下图所示：

![Alt text](CN5%E6%8A%93%E5%8C%85%E5%AE%9E%E9%AA%8C%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9.assets/image-8-1.png)


## lab7-arp

### 01 关于本机Ethernet地址的获取

我们大多使用的是无线网络（Wi-Fi），注意此时的Ethernet地址为如下图所示的`无线局域网适配器 WLAN`的物理地址：

![Alt text](CN5%E6%8A%93%E5%8C%85%E5%AE%9E%E9%AA%8C%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9.assets/image-10.png)

### 02 关于本机默认网关的地址

使用命令查询即可，如下图所示：

![Alt text](CN5%E6%8A%93%E5%8C%85%E5%AE%9E%E9%AA%8C%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9.assets/image-11.png)

### 03 Wireshark过滤器设置

在Step2中，使用如下过滤器：
```shell
arp && eth.addr==xx-xx-xx-xx-xx-xx
```
(注意，此处的`xx-xx-xx-xx-xx-xx`为本机Ethernet地址)

## lab8-ethernet

### 01 `Link-layer header`选项设置位置

选项位置如下图所示：
![Alt text](CN5%E6%8A%93%E5%8C%85%E5%AE%9E%E9%AA%8C%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9.assets/image-12.png)

### 02 组播Wireshark过滤器设置

在Step5中，需要设置组播过滤器。

实验文档中`ether multicast`过滤器语法不能生效，而应该用`eth[0] & 1 != 0`来过滤多播帧。

```shell
eth[0] & 1 != 0
```
