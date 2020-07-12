# 一 概述:
## (1)概述:
- 内核空间进程和用户空间进程的通信方式之一, 是网络应用程序和内核通信的首选方式, iproute2包大部分命令都是要该接口.
- netlink是面向报文的服务, netlink消息由一个包含一个或多个nlmsghdr头部的字节楼和相关的负载组成.

## (2)备注:
- man 3/7 netlink
- net/netlink
- man 3/7 rtnetlink
- net/core/rtnetlink.c
- 对linux而言, netlink就是BSD中的路由套接字.

# 二 用法:
## (1)概述:
- socket(AF_NETLINK, socket_type, netlink_family)
- socket_type: SOCK_RAW和SOCK_DGRAM.

## (2)netlink:
- NETLINK_ROUTE
- NETLINK_NETFILTER
- NETLINK_FIREWALL
- NETLINK_INET_DIAG
- 等等.