

# RS485方式控制背景音主机
分以下两种方式
  -  使用485线连接，发送指令
  -  使用udp无线，发送指令

## 485指令协议
支持两种协议

[声必可智能语控背景音乐系统RS485控制协议](./files/声必可智能语控背景音乐系统RS485控制协议.pdf)

[兼容华尔思背景音乐系统RS485控制协议](./files/兼容华尔思背景音乐系统RS485控制协议.pdf)


## 使用udp无线，发送指令
### 1. 通过DLNA设备发现发现背景音主机设备, 获得背景音主机IP地址。或者通过其它方式获取背景音主机IP地址
### 发现协议

设备发现采用DLNA的设备发现机制。Client加入组播组并发送SSDP M-SEARCH包到组播地址239.255.255.250:1900端口，也可以直接对当前局域网的广播地址(如192.168.1.255)直接发送SSDP M-SEARCH广播包。参考M-SEARCH包数据如下：
```
M-SEARCH * HTTP/1.1
MX: 2
ST: upnp:rootdevice
MAN: "ssdp:discover"
User-Agent: UPnP/1.0 DLNADOC/1.50 JdPlay/1.0.0
Connection: close
Host: 239.255.255.250:1900
```

Client监听到SSDP NOTIFY包，解析包如果含有EXT字段并该字段含有`JDPLAY`关键字，则发现背景音系统，如果版本号大于XXX,则可以通过本协议来控制。 参考NOTIFY包数据如下：
```
NOTIFY * HTTP/1.1
Host: 239.255.255.250:1900
Location: http://192.168.1.154:1500/
Cache-Control: max-age=100
Server: UPnP/1.0 DLNADOC/1.50 EglPlay/1.0.0
BOOTID.UPNP.ORG: 1490004021
EXT: JDPLAY/2.1.1
NTS: ssdp:alive
USN: uuid:10000003816::urn:schemas-upnp-org:device:MediaRenderer:1
NT: urn:schemas-upnp-org:device:MediaRenderer:1
```
### 2. 同背景音乐主机IP地址的固定端口28998， 建立udp socket链接， 并进行通信。
