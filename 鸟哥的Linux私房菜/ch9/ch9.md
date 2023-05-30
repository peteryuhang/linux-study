## Linux 系统常见的压缩命令

```
*.Z              -> compress 程序压缩的文件
*.gz             -> gzip 程序压缩的文件
*.bz2            -> bzip2 程序压缩的文件
*.tar            -> tar 程序打包的数据，并没有压缩过
*.tar.gz         -> tar 程序打包的数据，经过 gzip 的压缩
*.tar.bz2        -> tar 程序打包的数据，经过 bzip2 的压缩
```

### gzip,zcat

- 目前 gzip 可以解开 **compress**、**zip** 与 **gzip** 等软件所压缩的文件
- gzip 所新建的压缩文件为 *.gz 的文件名

```bash
>$ gzip [-cdtv#] 文件名
>$ gzip -v man.config                        # 压缩并显示压缩比等信息
>$ gzip -d man.config.gz                     # 将上面的文件解压缩
>$ gzip -9 -c man.config > man.config.gz     # 用最佳压缩比压缩，并保留原来的文件
```

- zcat 可以读取纯文本被压缩后的压缩文件

```bash
>$ zcat man.config.gz
```

### bzip2,bzcat

- gzip 是为了替代 compress 并提供更好的压缩比而成立的。bzip2 则是为了取代 gzip 并提供更佳的压缩比而来的
- bzip2 的用法几乎和 gzip 相同
- bzip2 可以解开下列类型的文件
  - .bz
  - .bz2
  - .tbz
  - .tbz2
- `bunzip2` 可以替代 `bzip2 -d`

```bash
>$ bzip2 [-cdtv#] 文件名
>$ bzip2 -z man.config                        # 压缩并显示压缩比等信息
>$ bzip2 -d man.config.bz2                    # 将上面的文件解压缩
>$ bzip2 -9 -c man.config > man.config.bz2    # 用最佳压缩比压缩，并保留原来的文件

>$ bcat man.config.bz2                        # 类似 cat 读出文件内容
```

### tar

```bash
# -j 通过 bzip2 的支持压缩/解压缩
# -z 通过 gzip 的支持压缩/解压缩
>$ tar [-j|-z] [cv] [-f 新建的文件名] filename...          # 打包与压缩
>$ tar [-j|-z] [tv] [-f 新建的文件名] filename...          # 查看文件名
>$ tar [-j|-z] [xv] [-f 新建的文件名] [-C 目录]             # 解压缩，在特定目录打开
```

例子：
```bash
>$ tar -jcv -f /root/etc.tar.bz2 /etc          # 将 /etc 打包并通过 gzip 压缩
>$ tar -jtv -f /root/etc.tar.bz2               # 查看 tar 文件的数据内容（文件名）
>$ tar -jxv -f /root/etc.tar.bz2 -C /tmp       # 在 /tmp 目录解开文件
>$ tar -jcv -f /root/system.tar.bz2 --exclude=/root/etc* --exclude=/root/system.tar.bz2 /etc /root # 打包某目录，但不包含该目录下的某些文件
>$ tar -cvf - /etc | tar -xvf -                # 一边打包一边解包，类似 cp -r /etc .
```

### dump

- 可针对整个文件系统，或是特定目录进行备份

```bash
>$ dump [-Suvj] [-level] [-f 备份文件] 待备份数据
```

例子:
```bash
>$ dump -S /dev/sda1                            # 测试一下如果备份此文件需多少容量
>$ dump -0u -f /root/boot.dump /boot            # 将完整备份的文件名记录成为 /root/boot.dump，同时更新记录文件
                                                # dump 时间记录到 /etc/dumpdates
>$ dump -0j -f /root/etc.dump.bz2 /etc          # 将整个 /etc 目录进行备份，并使用 bzip2 进行压缩
>$ dump -W                                      # 列出在 /etc/fstab 里面的具有 dump 设置的分区是否有备份过
```

### restore

- dump 的恢复使用

```bash
>$ restore -t [-f dumpfile] [-h]                # 用来查看 dump 文件
>$ restore -C [-f dumpfile] [-D 挂载点]          # 比较 dump 与实际文件
>$ restore -i [-f dumpfile]                     # 进入互动模式
>$ restore -r [-f dumpfile]                     # 还原整个文件系统
```

### dd

- 主要用作备份，还可以用来制作空文件
- dd 可以读取磁盘设备的内容（几乎是直接读取扇区）

```bash
>$ dd if="input file" of="output file" bs="block size" count="number"
```

例子：
```bash
>$ dd if=/etc/passwd of=/tmp/passwd.back                # 将 /etc/passwd 备份到 /tmp/passwd.back 中
>$ dd if=/dev/sda of=/tmp/mbr.back bs=512 count=1       # 将这个磁盘的 MBR 与分区表进行备份
>$ dd if=/dev/zero of=/tmp/swap bs=1M count=128         # 新增一个 128MB 的文件在 /tmp 下面
```

### cpio

- cpio 可用来备份任何东西，但是 cpio 得要配合类似 find 等可以找到文件名的命令来告知 cpio 该被备份的数据在哪里

```bash
>$ cpio -ovcB > [file|device]         # 备份
>$ cpio -ivcdu < [file|device]        # 还原
>$ cpio -ivct < [file|device]         # 查看
```

例子：
```bash
>$ find /boot | cpio -ocvB > /tmp/boot.cpio      # 通过 find 找到 /boot 下面存在的文件名，然后通过 cpio 对其进行备份
>$ cpio -idvc < /tmp/boot.cpio                   # 将上面备份的文件在当前目录下解开
```
