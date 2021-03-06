# 一 概述:
## (1)概述:
- 内核必须完成两种主要的时间测量: (1)**实时时钟**: 保存当前时间和日期, 以便通过time()等系统调用来返回给用户程序, 也可由内核本身把当前时间作为文件和网络包的时间戳; (2)**系统定时器**: 维持定时器, 告诉内核或应用程序(例如alarm系统调用)某一时间间隔已过去.
- 时间测量是基于固定频率振荡器和计数器等几个硬件电路完成的.

## (2)相关变量:
- HZ: 与CPU的hz不同, 不同体系结构不同, 常见为1000HZ. 其它: USER_HZ.
- Tick: HZ的倒数表示的时间间隔.
- jiffies: 记录系统自启动以来产生的Tick的总数.
- xtime: 当前实际时间(clock time), 定义在kernel/timer.c, 存放着自1970.7.1以来经过的时间, 每个节拍(定时器(timer)中断)更新一次.

## (3)相关文件:
- kernel/time/timer.c
- kernel/time/hrtimer.c

# 二 相关硬件电路:
## (1)RTC(实时时钟):
- Linux使用RTC来获取时间和日期(放在xtime变量), RTC能在IRQ8上发出周期性的中断(频率2-8192HZ), 也可以对RTC进行编程使得RTC到达某个特定值时激活IRQ8线(作为定时器).
- 独立于CPU和其它芯片, 即使PC被切掉电源, RTC还能继续工作, 独立于CPU和其它芯片, 靠主板上蓄电池供电.

## (2)TSC(时间戳计数器):
- 当时钟信号到来时加1, 例如**时钟频率(Clock Rate, 即CPU主频)**是1GHZ, 则每纳秒tsc增加一次.
- 与PIT对比, tsc可以获得更精确的时间测量, 内核在初始化时必须确定时钟信号的频率, 算出cpu实际频率的任务是在系统初始化时期完成的. 

## (3)PIT(可编程间隔定时器):
- 通过向内核以**固定频率**发出**定时器中断**(timer interrupt)来通知内核一段时间过去.
- 时钟中断的频率依赖硬件体系结构, 该频率是静态预定义的, 就是HZ, 在系统启动时根据HZ值对硬件进行设置, 体系结构不同, HZ的值不同, 在arch/xxx/include/asm/param.h.
- 允许在编译内核时选择不同的HZ值.
- HZ值高的优点: 更高的时钟中断解析度; 提高事件驱动实践的准确度; 依赖定时值的系统调用更高的精度运行(例如:select和poll); 提高进程抢占的准确度.
- HZ值高的缺点: 时钟中断越频繁, 处理器用来执行时钟中断处理程序的时间越多, 系统负担越重.


# 三 Linux计时体系结构.