# 一 概述：
## (1)相关概念:
- **rootfs**: 在系统boot阶段直接被kernel挂载, 包含系统初始化脚本和大量必须的系统程序.
- 其它文件系统被初始化脚本或者其它方式挂载.
- 每个文件系统有自己的**root directory**, 文件系统被挂载的目录称作:**挂载点(mount point)**.
- 一个被挂载系统是它的挂载点所在文件系统的child, 例如: /proc是rootfs的child, 被挂载系统的root目录隐藏(hide)了父文件系统中挂载点目录的内容.

## (2)创建文件系统：
- 使用fdisk创建磁盘分区(disk partition).
- 使用mkfs基于分区创建文件系统.
- 使用mount挂载次块设备(通常磁盘分区，例如:/dev/hda2)上已存在的文件系统到linux目录结构.

## (3)相关命令：
- fdisk: 硬盘分区工具.
- mkfs/mke2fs: 对磁盘分区格式化, 创建文件系统.
- fsck：检查和修复文件系统.
- mount: 挂载device中包含文件系统到指定目录.
- umount
- badblocks

## (4)增加硬盘的步骤:
- 磁盘分区: 对磁盘进行分区,fdisk等.
- 磁盘格式化: 对磁盘分区进行格式化, mkfs等.
- 磁盘检查(可选): 对刚创建的文件系统进行检查, fsck, badblocks等.
- 磁盘挂载和卸载: 创建挂载点, 并挂载文件系统, mount和umount.

## (5)VFS支持文件系统分类:
- **基于磁盘的文件系统**: 管理本地磁盘或其他模拟磁盘的设备(USB)的空间, 常见: Ext2-4, MS-DOS等等.
- **网络文件系统**: 允许访问网络中其他机器上的文件, 例如: NFS, Coda, AFS等.
- **特殊文件系统**: 不管磁盘空间(本地或远程), 例如:proc, rootfs等.

## (6)备注: 
- /proc
- Documentation/filesystems
- /dev/zero: 特殊设备文件, 读取时会提供无限空字符, 常用来生产空白文件.
- /dev/null: 特殊设备文件, 会丢弃写入的一切数据.
- /proc/filesystems:存放当前内核支持的文件系统类型.
- /etc/fstab:当前系统中挂载的文件系统的信息.
- /mnt:通用的挂载文件系统或设备的目录.
- 文件系统被mount后才能正常使用.

# 二 文件系统类型:
## (1)概述:
- file_system_type

## (2)类型:
- ext2-4
- xfs
- nfs
- rootfs
- ramfs
- sysfs
- proc
- 等等.

# 三 日志型文件系统
