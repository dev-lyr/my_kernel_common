# 一 概述：
## (1)简介：
- linux内核是模块化的, 允许在**运行时**插入和删除相应的模块, 基本内核可尽可能小,可选功能和驱动程序可以以模块形式提供.
- ko(kernel module)：模块文件，在linux系统启动时加载内核模块.
- 系统模块运行在内核空间，应用程序运行在用户空间.

## (2)模块操作相关包：
- module-init-tools
- kmod：取代module-init-tools.

## (3)相关命令(module-init-tools)：
- lsmod：显示内核中所有模块，/proc/modules.
- modinfo：显示一个模块的信息.
- insmod和rmmod：添加和删除内核模块，单独模块.
- modprobe：添加和删除内核模块，会自动添加和删除依赖.
- depmod：产生一个模块依赖文件，根据/lib/modules/version下的模块产生该文件，名字为modeles.dep，在同样目录下.

## (4)相关文件:
- include/linux/module.h
- include/linux/export.h

# 二 modprobe：
## (1)概述：
- modprobe使用/etc/modprobe.conf文件来推断如何处理每个模块.

# 三 编写内核模块：
## (1)概述：
- module_init和module_exit内核宏：指定模块被加载和卸载时调用的函数.
- MODULE_LICENSE宏：告诉内核模块的自由许可证.
- MODULE_AUTHOR宏：指定作者.
- EXPORT_SYSBOL：将内核默认非static的函数和变量导入到内核空间，2.6之后才出现，以前是默认导出的.
- EXPORT_SYSBOL_GPL
