## Linux 文件与目录管理

### 目录的相关操作

#### 相关指令

```bash
>$ cd                     # 等同于 cd ~ 表示回到自己的主文件夹
>$ cd -                   # 回到刚才那个目录
>$ pwd [-P]               # 列出当前的工作目录，带上 -P 显示出当前路径，而非连接（link）
>$ mkdir [-mp] dirname    # 创建目录，-m 表示设置权限，-p 帮助将目录递归创建起来
>$ rmdir [-p] dirname     # 删除目录，-p 表示连空的目录一同删除
```

#### $PATH

- 不同身份用户默认的 PATH 不同，默认能够随意执行的命令也不同
- PATH 是可以修改的，所以一般用户可以修改 PATH 来执行某些目录下的命令
- 直接指定某个命令的文件名来执行，会比查询 PATH 来得准确
- 本目录 `.` 最好不要放到 PATH 当中（主要为了安全考虑）

### 文件与目录管理

几个相关的指令，以及重要的选项，详细可通过 **man** 查看

```bash
>$ ls [-aAdfFhilnrRSt,--full-time] dirname
>$ cp [-adfilprsu] src1 src2 ... dest
>$ rm [-fir] filepath/dirname
>$ mv [-fiu] src1 src2 ... dirname
>$ basename
>$ dirname
```

### 文件内容查阅

#### 直接查看文件内容

```bash
>$ cat [-AbEnTv]           # 由第一行开始显示文件内容
>$ tac                     # 从最后一行开始显示文件内容
>$ nl [-bnw]               # 显示的时候顺便输出行号
>$ more                    # 一页一页地显示文件内容
>$ less                    # 与 more 类似，但是比 more 更好的是，它可以往前翻页
>$ head [-n number]        # 只看头几行
>$ tail [-n number][-f]    # 只看结尾几行，带上 -f 表示持续监测，常用于查看 log 文件
>$ od                      # 以二进制的方式读取文件内容
```

- Linux 系统下会记录多个时间参数：
  - modification time(mtime) 默认显示时间
  - status time(ctime)
  - access time(atime)
  - eg. `ls -l --time=atime ~` 

```bash
>$ touch [-acdmt] file         # touch 用来创建新文件，但同时也可以用来修改文件时间
```

### 文件目录的权限

#### 文件默认权限：umask

- `umask` 就是指定 “目前用户在新建文件或目录时的权限默认值”
- `umask` 的分数指的是 “该默认值需要减掉的权限”

#### 文件隐藏属性 chattr，lsattr

```
>$ chattr [+-=][ASacdistu] filename/dirname
>$ lsattr [-adR] filename/dirname
```

#### 文件特殊权限：SUID，SGID，SBIT

- SetUID
  - 权限状态 `-rwsrwxrwx`(`-rwSrwxrwx` 表示空权限，因为 x 没有设置)
  - SUID 权限 **仅对二进制程序** 有效，不能够用在 shell script 上面
  - 执行者对于该程序需要具有 x 的可执行权限
  - 本权限仅在执行该程序的过程中（run-time）有效
  - **执行者将具有该程序所有者（owner）的权限**
- SetGID
  - 权限状态 `-rwxrwxrwx`(`-rwxrwSrwx` 表示空权限，因为 x 没有设置)
  - 与 SUID 类似，程序执行者将具有获得该程序用户组的支持
- Sticky Bit
  - 权限状态 `drwxrwxrwt`(`drwxrwxrwT` 表示空权限，因为 x 没有设置)
  - 只针对目录有效
  - 当用户对此目录有 `w,x` 权限时，用户在该目录下创建的文件或目录 **仅有自己与 root** 才有权删除，重命名以及移动等操作
  - 通常 `/tmp` 目录被设置成 SBIT

- SUID/SGID/SBIT 权限设置
  - 4 为 SUID
  - 2 为 SGID
  - 1 为 SBIT

#### 查看文件类型：file

- 告诉我们一个文件是属于 ASCII 或者是 data 文件
- `>$ file ~/.bashrc`

### 命令与文件的查询

```bash
# 寻找 “执行文件”，which 是默认查找 PATH 内所规范的目录
# -a 表示 PATH 目录中可以找到的命令均列出，而不只是第一个找到的
>$ which [-a] command

# 利用数据库来查找数据，比 find 要快，一般都是 whereis 和 locate 找不到了再用 find
>$ whereis [-bmsu] filename/dirname
# 在以创建的数据库 /var/lib/mlocate/ 里查找
# 需要用 updatedb 手动更新数据库
>$ locate [-ir] keywork
```

find 命令具体案例
```bash
# 将过去系统上面 24 小时内有更劢过内容 (mtime) 的档案列出
>$ find / -mtime 0

# 寻找 /etc 底下的档案，如果档案日期比 /etc/passwd 新就列出
>$ find /etc -newer /etc/passwd

# 搜寻 /home 底下属于 vbird 的档案
>$ find /home -user vbird

# 搜寻系统中不属于任何人的档案
>$ find / -nouser

# 搜寻档案中含有 SGID 或 SUID 或 SBIT 的属性
>$ find / -perm /7000

# 将上个范例找到的档案使用 ls -l 列出来
>$ find / -perm +7000 -exec ls -l {} \;

# 找出系统中，大于 1MB 的档案
>$ find / -size +1000k

# 你想要找出 /etc 底下档案名包含 httpd 的档案
>$ find /etc -name '*httpd*'
```

![](Screen%20Shot%202023-04-22%20at%201.02.10%20PM.png)
