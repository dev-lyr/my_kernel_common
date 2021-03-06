# 一 概述:
## (1)常用技术:
- bpf(ebpf)
- kprobe
- linux perf
- systemtap
- ftrace
- gprof
- kgdb

## (2)前端工具:
- 对底层技术进行封装, 更方便用户使用.
- 例如: systemtap(基于kprobe),perf,bcc(基于ebpf)等.

## (3)备注:
- http://www.brendangregg.com/overview.html
- https://www.joyfulbikeshedding.com/blog/2019-01-31-full-system-dynamic-tracing-on-linux-using-ebpf-and-bpftrace.html

# 二 trace:
## (1)类型:
- static tracing: Hard-coded instrumentation points in code.
- dynamic tracing: dynamic instrumentation, this is a technology that can instrument any software event, such as function calls and returns, by live modification of instruction text.

## (2)动态tracing:
- kprobes(Kernel Probes)
- uprobes(user-level dynamic tracing)

## (3)静态tracing:
- tracepoints
- USDT(User Statically-Defined Tracing)

## (4)相关frontends工具:
- systemtap
- ftrace

## (5)备注:
- Documentation/trace
