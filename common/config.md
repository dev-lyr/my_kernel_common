# 一 sysctl：
## (1)概述:
- sysctl命令
- _sysctl函数
- /etc/sysctl.conf: 永久生效.
- /proc/sys/*: 重启后失效.

## (2)相关:
- Document/sysctl/*.txt: 配置的功能和意义.
- Document/kernel-parameters.txt.
- kernel/sysctl.c和kernel/sysctl_binary.c(所有配置变量的层次结构).

## (3)备注:
- sysctl不能对每个内核变量进行操作, 内核应明确指定哪些变量此接口可见.
- 注意进程和系统级别的一些配置和限制.
- sysctl.conf的变量根据.号分隔映射到/proc/sys/下的文件.

# 二 进程级别限制:
## (1)系统调用:
- getrlimit
- setrlimit

## (2)ulimit:
- ulimit: 限制当前shell进程及其子进程, 对其它进程无影响.

## (3)soft和hard limit:
- soft limit: 内核对某种资源的强制限制.
- hard limit: 表示soft limit的上限, 非特权进程只能设置soft limit在0~hard limit之间, 特权进程可以人工修改配置.

