# 一 dev_queue_xmit:
## (1)功能:
- 调用设备驱动程序的hard_start_xmit发送帧.
- 当无法发送时, 比如设备出口队列已经关闭或锁被抢走, 则把设备添加到output_queue由软中断来发送包.

# 二 net_tx_action:
## (1)功能:
- 当网络设备传输功能开启时, 所有发送条件满足时, 将帧发送出去.
- 当传输完成时设备通过dev_kfree_skb_irq通知相关缓冲区可释放时, 回收已成功传输的sk_buff结构.