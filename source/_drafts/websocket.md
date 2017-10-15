---
title: websocket
layout: fasle
date: 2016-10-21 17:10:54
updated:
comments:
tags:
---




# TCP/IP协议

## 每层协议的数据包
TCP(Transport layer 传输层) -- Segment[报文] --   port
IP(Network layer 网际层) -- Datagram or Packet [数据报]  -- ip
Ethernet(Link layer链路层)==> Frame[帧] -- mac
TCP -  MSS(maximum segment size) 最大消息长度
IP - 

## 分层模型下与通信示例

### 数据链路的抽象化



### IP的分割处理 和 再构成处理

*HttpData*TcpHeader*IpHeader 分包为
*HttpData_part1*TcpHeader*IpHeader
*HttpData_part2*TcpHeader*IpHeader
*HttpData_part3*TcpHeader*IpHeader







10.0.0.0  ~ 10.255.255.255
00001010.0.0.0  ~ 00001010





