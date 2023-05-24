## EXT 文件系统

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
  - inodetable
    - inode 的结构图

    ![](Screen%20Shot%202023-05-24%20at%2010.12.29%20AM.png)
  - superblock
    - 记录整个文件系统相关信息的地方，记录的信息有
      - block 与 inode 的总量
      - 未使用与已使用的 inode/block 数量
      - block 与 inode 的大小
      - 文件系统的挂载时间、最近一次写入的时间、最近一次检验磁盘（fsck）的时间等文件系统的相关信息
      - 一个 validbit 数值，若此文件系统已被挂载，则 valid bit 为 0，若未被挂载，则 valid bit 为 1