## Linux 的历史

- 1960, 分时操作系统(Compatible Time-Sharing System, CTSS)
  - 终端机 -> 主机，终端机只有输入输出的功能，并无计算功能
- 1969, Multics，目的是让主机可以提供更过的终端机连接
- 1969, Ken Thompson 用汇编写出了 Unics，一个文件系统，有 **两个重要概念**
  - 所有的程序或系统装置都是文件
  - 不管构建编辑器还是附属文件，所写的程序只有一个目的，就是要有效地完成目标
- 1973, Ritchie 等人以 C 语言写出第一个正式 UNIX 内核
- 1977, 重要的 UNIX 分支 - BSD（Berkeley Software Distribution）诞生
- 1979, 重要的 System V 架构与版权声明
- 1984, x86 架构的 Minix 操作系统诞生
- 1984, GNU (GNU's NOT Unix) 项目与 FSF (Free Software Foundation) 成立
  - GNU 的目的是创建一个自由、开放的 UNIX 操作系统
  - GNU 的通用公共许可证（General Public License, GPL）, 并且称它为 CopyLeft
  - GNU 在当时开发的几款重要软件
    - Emacs
    - GNU C (GCC)
    - GNU C Library (GLIBC)
    - Bash shell
- 1988, 图形接口 XFree86 (X Window System + Free + x86) 项目
- 1991, Linus Torvalds 以 bash, gcc 等工具写了一个小小的内核程序 (Linux)

## Linux 的开发

- 由于希望 Linux 兼容 Unix，Torvalds 参考 POSIX (Portable Operating System Interface) 规范，这个规范了应用程序与操作系统之间的接口
- Linux 的内核版本（注意：内核版本不是 distribution 版本，如 Ubuntu）
  - 主版本.次版本.释出版本-修改版本 (eg. 2.6.18-92.e15)
  - 主、次版本为奇数：开发中版本（development）
  - 主、次版本为偶数：稳定版本（stable）
- distribution = Kernel + Softwares + Tools
- 所有的 Linux distribution（eg. Ubuntu, CentOS etc）使用的都是 Linux 内核，不过每家 distributions 所选用的软件和工具并不相同
- 为了让 Linux distribution 的差异不至于太大，有些标准来约束
  - Linux Standard Base（LSB）
  - File System Hierarchy Standard（FHS）
- distribution 主要分为两大系统
  - 使用 RPM 方式安装软件的系统（eg. Red Hat、Fedora、SuSE）
  - 使用 Debian 的 dpkg 方式安装软件的系统（eg. Debian、Ubuntu、B2D）
- 可以使用 `uname -r` 命令查看 Linux 内核版本
