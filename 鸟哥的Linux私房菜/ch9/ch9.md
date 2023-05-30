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


