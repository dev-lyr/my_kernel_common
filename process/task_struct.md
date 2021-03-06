# 一 概述:
## (1)概述:
- 进程描述符由task_struct(include/linux/sched.h)类型结构表示，包含于一个进程相关的所有信息.
- 进程和进程描述符是一一对应关系, 即使轻量级进程(线程)也有自己的task_struct结构.

# 二 task_struct:
## (1)常用字段:
- pid_t pid: 进程ID, PID被顺序编号，新建进程通常是前一个进程的PID加1，PID的大小有上限(PID_MAX_DEFAULT和PID_MAX_LIMIT), 当达到上限后就必须开始循环使用已闲置的小的PID. cat /proc/sys/kernel/pid_max.
- pid_t tgid: 使用线程组的情况下，领头线程的pid会存入其它线程的tgid字段，这些线程getpid返回的就是tgid字段，而不是pid字段，领头线程的tgid和pid一致, 这么做的目的是希望同一线程组中线程有相同的PID.

## (2)状态相关:
- long state
- int exit_state
- int exit_code, exit_signal

## (3)进程间关系:
- real_parent：创建该进程的进程的进程描述符，若该进程不再存在，则指向进程1(init)的描述符.
- children：该进程创建的孩子进程.
- sibling：兄弟进程.
- parent.
- group_leader: 进程组领头进程的进程描述符.

## (4)文件系统和文件相关:
- files：描述符表. clone的CLONE_FILES控制父子进程间是否共享.
- fs: 记录进程的root目录和当前工作目录, root目录可通过chroot修改, 当前工作目录可通过chdir修改.

## (5)内存相关:
- mm:进程所拥有的内存描述符, 存放与进程地址空间有关的全部信息.
- active_mm.
- 备注:普通进程两个字段存放相同的指针；由于内核线程没有任何内存描述符,则内核线程的mm总为NULL.

## (6)上下文切换相关:
- struct thread_struct thread: 存放当前进程cpu相关的状态, 进程被切换出去时, 内核将其硬件上下文保存在该字段, 该字段保存在内核堆栈中.

# 三 struct thread_info
