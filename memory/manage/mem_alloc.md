# 一 分配方式:
## (1)连续分配方式:
- **固定分区(连续分区)**: 将内存用户空间划分为多个固定大小的静态分区，进程被装入大于或等于自身大小的分区.
- 分区的大小可以是长度都相等；或者不等，分为多个较小分区、适量中等分区，少量较大分区.
- 优点: 实现简单，开销小.
- 缺点: 有内部碎片，内存使用不充分；活动进程的最大数目是固定的.
- **动态分区(连续分区)**: 分区是动态创建的，每个进程可以被装入与自身大小相等的分区中.
- 优点: 没有内部碎片；可以充分使用内存.
- 缺点: 有外部碎片，由于需要压缩外部碎片，处理器利用率低.

## (2)离散分配方式:
- **简单分页**: 内存被划分为许多大小相等的页框；每个进程被划分成许多大小与页框大小相等的页；要装入进程，需要把进程的所有页都装入内存中不一定连续的某些页框中.
- 优点: 没有外部碎片.
- 缺点: 有少量内部碎片.
- **简单分段(段的大小不固定)**: 每个进程被划分成许多段；要装入进程需要把进程包含的所有段都装入内存中不一定连续的某些动态分区中.
- 优点: 没有内部碎片，相对于动态分区，提高了内存利用率，减少了开销.
- 缺点: 存在外部碎片.
- **虚拟内存分页**: 除了不需要把进程的所有页都装入内存之外，与简单分页一样；非驻留页在以后需要时调入内存.
- 优点: 没有外部碎片；巨大的虚拟地址空间.
- 缺点: 复杂的内存管理开销.
- **虚拟内存分段**: 除了不需要把进程的所有段都装入内存之外，与简单分段一样；非驻留段在以后需要时调入内存.
- 优点: 没有外部碎片；巨大的虚拟地址空间；支持共享和保护.
- 缺点: 复杂的内存管理开销.

# 二 内核内存分配:
## (1)概述:
- 内核是os中优先级最高的部分,若某个内核函数请求内存, 那么必定是有正当理由, 因此需立即分配.
- 内核信任自己, 所有内核函数都被假定是没有错误, 因此内核函数没有对编程错误的保护.

## (2)分配和释放页框(以页为单位, linux/gfp.h):
- alloc_pages: 分配几个连续的物理页(物理上连续), 并返回一个page结构指针指向第一个页.
- alloc_page: 分配单个物理页.
- page_address: 返回给物理页当前所在环境的逻辑地址.
- __get_free_pages: 与alloc_pages类似只是返回的是第一个页的逻辑地址.
- __get_free_page: 同上, 只不过是单页.
- get_zeroed_page: 与__get_free_pages工作方式相同, 只是把所有页面都填充为0.
- 释放: __free_pages, free_pages和free_page.

## (3)字节为单位分配和释放(内核内存):
- kmalloc/kfree: 获得/释放以字节为单位的一块内核内存, 在物理上是连续的.
- vmalloc/vfree: 与kmalloc类似, 只不过vmalloc是虚拟地址是连续的, 物理地址无需连续, 非连续物理内存, 类似用户空间函数的工作方式.
- 备注: 很多内核代码直接使用kmalloc, 因为kmalloc的性能比vmalloc好.

## (4)相关系统:
- **伙伴系统**: 一种健壮、高效的分配策略,用来解决**外部碎片**问题,以**页框**为分配单元, 适合大内存需求.
- **slab算法**: 用来解决**内部碎片**问题, 以**字节**为分配单元.

# 三 用户内存分配(虚拟地址):
## (1)概述:
- 详细参见进程的地址空间.
- 内核认为用户态进程的动态内存分配是不紧迫, 因此总是尽量推迟给用户态进程分配内存.
- 对用户态进程是不信任, 因此内核必须随时准备捕获用户态进程引起的所有寻址错误.
- 当用户态进程请求动态内存时, 并没有获得请求的页框, 仅仅是获得对一个新的线性地址区间的使用权，缺页异常处理程序在当进程访问该地址时真正给进程分配页框.

## (3)字节为单位分配和释放(用户空间内存):
- malloc, calloc,realloc和free.
- brk(malloc调用):系统调用.
- sbrk:c库封装函数.
- 备注:brk与其他函数不同, 它是系统调用, 其他函数都是使用brk和mmap系统调用使用的C语言库函数.
- 代码:mm/mmap.c