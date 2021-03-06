# 一 概述：
## (1)拥塞控制(与流量控制的区别)：
- 拥塞控制：防止过多的数据注入到网络中，这样可以使网络中的路由器或链路不致过载。拥塞控制是一个全局性的过程。
- 流量控制：往往指点对点通信量的控制，是个端到端的问题。流量控制目的就是抑制客户端发送数据的速率，以便接收端来得及接受。

## (2)相关概念：
- 拥塞窗口(cwnd)：发送方维持一个拥塞窗口(congestion window)状态变量。拥塞窗口的大小取决于网络的拥塞程度，并且动态变化，**发送窗口=min(拥塞窗口，接收窗口).**

## (3)系统配置:
- tcp_congestion_control显示当前使用的拥塞避免算法. 例如：bic、cubic、bbr(kernel 4.9).

## (4)备注：
- 拥塞控制是一个动态的问题，拥塞控制很难设计.
- 这里的数字是报文而不是字节.
- TCP_CONGESTION socket选项.
- 源文件：net/ipv4/tcp_bic.c、net/ipv4/tcp_cubic.c、net/ipv4/tcp_bbr.c.

# 二 拥塞控制方法：
## (1)概述：
- 慢开始门限(ssthresh)：为了防止拥塞窗口增长过大而引起网络阻塞，还需要设置一个慢开始门限状态变量。
- 用法如下：当拥塞窗口小于慢开始门限时，采用慢开始算法；当拥塞窗口大于慢开始门限时，停止使用慢开始算法而采用拥塞避免算法；当拥塞窗口等于慢开始门限时，可以采用慢开始算法或拥塞避免算法。

## (2)慢开始算法(slow-start)：
- 开始时，设置拥塞窗口为1，发送第一个报文段M1，接收方收到后确认M1。发送方收到对M1的确认后，把拥塞窗口增大到2，于是发送M2和M3两个报文段。
- 接收方收到后发回对M2和M3的确认。发送方每收到一个对新报文段的确认，拥塞窗口就增加1。这样拥塞窗口就增大到4，并可发送M4~M7共四个报文段。因此，慢开始算法，每经过一个传输轮次(transmission round)，拥塞窗口就加倍。
- 慢开始的“慢”并不是指拥塞窗口增长慢，而是指TCP开始发送报文段时设计拥塞窗口为1，使得发送方在开始时只发送一个报文段(目的是试探一下网络的拥塞情况)，然后逐步增大拥塞窗口。

## (3)拥塞避免算法(congestion和avoidance)：
- 思路：让拥塞窗口慢慢增大，即每经过一个传输轮次就把发送方的拥塞窗口加1，而不是加倍。

## (4)备注：
- 无论在慢开始阶段还是在拥塞避免阶段，只要发送方判断网络出现拥塞，就要把慢开始门限设置为出现拥塞时的发送窗口值的一半，将拥塞窗口(发送窗口)置为1.

# 三 快重传和快恢复：
## (1)快重传：
- 要求接收方每收到一个失序的报文段后就立即发出重复确认而不是等待自己发送数据时才捎带确认
- 发送方只要连续收到三个重复确认就应当立即重传尚未收到的报文段，而不必等待重传计时器到期

## (2)快恢复(与快重传配合使用，过程如下)：
- 当发送方连续收到三个重复确认时，就执行“乘法减小”算法，把慢开始门限减半。
- 由于发送方现在认为网络很可能没有发送拥塞(如果发生严重拥塞，就不会有一连好几个报文段连续到达接收方)，因此，现在不执行慢开始算法(即拥塞窗口不设置为1)，而把拥塞窗口设置为慢开始门限减半后的值，然后开始执行拥塞避免算法(“加法增大”).