# 一 概述:
## (1)概述:
- IP sets是linux2.4.x(以及更新内核版本)中的一个框架, 通过ipset工具管理.
- 根据类型, 一个IP set可以存储IP addresses,networks,port numbers,MAC addresses, interface names或者它们的组合.

## (2)使用场景:
- store multiple IP addresses or port numbers and match against the collection by iptables at one swoop.
- dynamically update iptables rules against IP addresses or ports without performance penalty.
- express complex IP address and ports based rulesets with one single iptables rule and benefit from the speed of IP sets.

## (3)命令:
- ipset: ip set的管理工具.

## (4)备注:
- https://ipset.netfilter.org/
