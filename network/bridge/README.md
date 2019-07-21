# 一 相关概念:
## (1)未知单播:
- 交换机初始时，mac地址是空的，此时A发生一个帧到B，那么交换机接收到此帧时候，查看源地址(主机A)，并将它添加到MAC地址表，但是交换机不知道B在哪个端口(mac地址表中没有B的MAC地址)，所以这个帧就是未知单播帧，交换机会flooding这个帧.

## (2)广播和多播:
- 当网桥收到一个目的地址是链路层广播或多播地址的帧时，会将该帧复制给每个端口(接收该帧的端口除外).

## (3)定时器:
- 为了让网桥能适应拓扑的变化，网桥为每个地址项设定了一个定时器，当首次学到该地址时，定时器就启动，以后任何时候再次监听到该主机时，就重启定时器，以便确认和更新地址,默认的老化时间是5分钟.

## (4)Bridge flooding:
- 若数据帧中的目的地址不在交换机的MAC地址表中，则向所有端口转发.

## (5)vlan:
- 局域网网段构成的与物理位置无关的逻辑组，属于同一网段的一组端口，这些网段具有共同需求. 每个vlan的帧都有一个标示符，指示该帧属于哪个vlan. 交换机不向vlan之外的工作站发生广播信息，不同vlan间的通信需要路由器. 目录: /proc/net/vlan.
- 可解决广播风暴(broadcast storm).

# 二 brctl命令:
## (1)功能:
- linux内核中ethernet bridge的创建,维护和配置.
- 相关包: bridge-utils.
- 用法: brctl [command]

## (2)命令:
- `addbr <bridge>: add bridge.`
- `delbr <bridge>: delete bridge.`
- `addif <bridge> <device>: add interface to bridge.`
- `delif <bridge> <device>: delete interface from bridge.`
- `hairpin <bridge> <port> {on|off}: turn hairpin on/off.`
- `setageing <bridge> <time>: set ageing time.`
- `setbridgeprio <bridge> <prio>: set bridge priority.`
- `setfd <bridge> <time>: set bridge forward delay.`
- `sethello <bridge> <time>: set hello time.`
- `setmaxage <bridge> <time>: set max message age.`
- `setpathcost <bridge> <port> <cost>: set path cost.`
- `setportprio <bridge> <port> <prio>: set port priority.`
- `show [ <bridge> ]: show a list of bridges.`
- `showmacs <bridge>: show a list of mac addrs.`
- `showstp <bridge>: show bridge stp info.`
- `stp <bridge> {on|off}: turn stp on/off.`