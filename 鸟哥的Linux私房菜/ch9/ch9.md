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
>$ dump -W                                      # 列出在 /etc/fstab 里面的具有 dump 设置的分区是否有备份过
```



