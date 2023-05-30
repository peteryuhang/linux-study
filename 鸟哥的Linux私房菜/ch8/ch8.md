## EXT 文件系统要素介绍

- 文件系统的构成要素
  - **super block**：记录此文件系统的整体信息，包括 inode/block 的总量、使用量、剩余量，以及文件系统的格式与相关信息
  - **inode**：记录文件的属性，一个文件占用一个 inode，同时记录此文件的数据所在的 block 号码
  - **block**：实际记录文件的内容，若文件太大时，会占用多个 block
- EXT 文件为 **索引式文件系统（indexed allocation）**。FAT 格式则和 EXT 不一样，无 inode，每个 block 号码都记录在前一个 block 当中，但是这种格式常常因文件写入的过于离散，需要 **碎片整理**
- 文件系统的结构图:

  ![](Screen%20Shot%202023-05-24%20at%209.58.33%20AM.png)
  - **data block**
    - 由于 block 的大小的区别（有 1KB，2KB 及 4KB 三种），会导致该文件系统能够支持的最大磁盘容量与最大单一文件容量并不相同
    - block 的大小与数量在格式化完就不能够再改变了
    - 每个 block 内最多只能够放置一个文件的数据
    - 如果一个文件大于 block 的大小，则一个文件会占用多个 block 的数量
    - 如果一个文件小于 block 的大小，则该 block 的空间不能够被再利用
  - **inodetable**
    - inode 的结构图

    ![](Screen%20Shot%202023-05-24%20at%2010.12.29%20AM.png)
  - **superblock**
    - 记录整个文件系统相关信息的地方，记录的信息有
      - block 与 inode 的总量
      - 未使用与已使用的 inode/block 数量
      - block 与 inode 的大小
      - 文件系统的挂载时间、最近一次写入的时间、最近一次检验磁盘（fsck）的时间等文件系统的相关信息
      - 一个 validbit 数值，若此文件系统已被挂载，则 valid bit 为 0，若未被挂载，则 valid bit 为 1
  - **File system Description（文件系统描述说明）**
    - 这个区段可以描述每个 block group 的开始与结束的 block 号码，以及说明每个区段（super block，data block，inodemap，。。。）分别介于哪一个 block 号码之间
  - **block bitmap（块对照表）**
    - 从中可以知道哪些 block 是空的，因此我们的系统就能够很快速地找到可使用的空间来处置文件
    - 删除同理
  - **inode bitmap（inode 对照表）**
    - 与 block bitmap 类似，则是记录未使用的 inode 号码
  - 可以使用 `dumpe2fs` 指令查询有关文件系统的各种信息
- 文件系统与目录树的关系：
  - 在 Linux 下的 EXT 文件系统新建一个 **目录** 时，EXT 会分配一个 inode 与至少一块 block 给该目录
    - inode 记录该目录的相关权限与属性，并可记录分配到的那块 block 号码
    - block 记录在这个目录下的文件名与该文件占用的 inode 号码数据
  - 在 Linux 下的 EXT 文件系统新建一个 **文件** 时，EXT 会分配一个 inode 与相对于该文件大小的 block 数量给该文件。如果 block 数量过多，还会增加 block 用来给 inode 作为块号码的记录

## 文件系统的简单操作

- 磁盘的整体数据是在 superblock 块中，但是每个各别文件的容量则在 inode 当中记录

- `df`: 列出文件系统的整体磁盘使用量
  ```bash
  >$ df           # 将系统内所有的文件系统列出
  >$ df /         # 将根目录所在文件系统的信息列出
  >$ df -ih       # 将目前各个分区当中可用的 inode 数量列出
  ```
- `du`: 评估文件系统的磁盘使用量（常用于评估目录所占容量）
  ```bash
  >$ du           # 列出目前目录下的所有文件容量（目录容量并不包含子目录）
  >$ du -sm /*    # 检查根目录下面每个目录所占用的容量（目录容量包含子目录），容量单位是 MB（默认是 KB）
  ```
- `ln`: 创建链接文件
  - **hard link**: （硬链接或实际链接）
    - hard link 只是在某个目录下新建一条文件名连接到某 inode 号码的关联记录而已，换句话说，只是在某个目录下的 block 多写入一个关联数据而已，既不会增加 inode，也不会耗用 block 数量
    - 删除 hard link 并不会改变指向的原有文件，所以是 “安全” 的
    - 不能跨文件系统
    - 不能连接到目录
    - 删除源文件，连接文件正常开启（由于有连接文件，源文件并不会真正地被删除）
  - **symbolic link**: (符号连接，也即快捷方式)
    - 创建一个独立的文件（会占用掉 inode 与 block），而这个文件会让数据的读取指向它连接的那个文件的文件名
    - 当源文件被删除后，symbolic link 的文件会 “开不了”
  ```bash
  >$ ln [-sf] 源文件 目标文件
  >$ ln passwd passwd-hd                  # 创建实际链接
  >$ ln -s passwd passwd-so               # 创建符号链接
  ```