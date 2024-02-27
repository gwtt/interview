### 1.PING 命令的作⽤是什么？

> PING 命令是一种常用的网络诊断工具，经常用来测试网络中主机之间的连通性和网络延迟。

### 2.PING 命令的⼯作原理是什么？

> PING 基于网络层的 **ICMP（Internet Control Message Protocol，互联网控制报文协议）**，其主要原理就是通过在网络上发送和接收 ICMP 报文实现的。
>
> ICMP 报文中包含了类型字段，用于标识 ICMP 报文类型。ICMP 报文的类型有很多种，但大致可以分为两类：
>
> - **查询报文类型**：向目标主机发送请求并期望得到响应。
> - **差错报文类型**：向源主机发送错误信息，用于报告网络中的错误情况。
>
> PING 用到的 ICMP Echo Request（类型为 8 ） 和 ICMP Echo Reply（类型为 0） 属于查询报文类型 。
>
> - PING 命令会向目标主机发送 ICMP Echo Request。
> - 如果两个主机的连通性正常，目标主机会返回一个对应的 ICMP Echo Reply。
