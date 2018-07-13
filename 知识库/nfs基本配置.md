## NFS

NFS（Network File System）即网络文件系统，是FreeBSD支持的文件系统中的一种，它允许网络中的计算机之间通过TCP/IP网络共享资源。在NFS的应用中，本地NFS的客户端应用可以透明地读写位于远端NFS服务器上的文件，就像访问本地文件一样。

### 搭建NFS服务器

#### 安装

CentOS默认已经安装NFS服务。可以通过' rpm -qa |grep nfs '查看。

#### 配置

常用配置文件

```
/etc/exports                           NFS服务的主要配置文件
/usr/sbin/exportfs                     NFS服务的管理命令
/usr/sbin/showmount                    客户端的查看命令
/var/lib/nfs/etab                      记录NFS分享出来的目录的完整权限设定值
/var/lib/nfs/xtab                      记录曾经登录过的客户端信息
```

NFS服务的配置文件为 /etc/exports，这个文件是NFS的主要配置文件，不过系统并没有默认值，所以这个文件不一定会存在，可能要使用vim手动建立，然后在文件里面写入配置内容。

/etc/exports文件内容格式：

```
<输出目录> [客户端地址1 选项（访问权限,用户映射,其他）]  [客户端地址2 选项（访问权限,用户映射,其他）]
```

示例：/home/static  * (rw,no_root_squash,async)

> 客户端地址

```
指定ip地址的主机：192.168.0.200
指定子网中的所有主机：192.168.0.0/24 192.168.0.0/255.255.255.0
指定域名的主机：david.bsmart.cn
指定域中的所有主机：*.bsmart.cn
所有主机：*
```

> 选项

NFS主要有3类选项：

访问权限选项

```
设置输出目录只读：ro
设置输出目录读写：rw
```

用户映射选项

```
all_squash：将远程访问的所有普通用户及所属组都映射为匿名用户或用户组（nfsnobody）；
no_all_squash：与all_squash取反（默认设置）；
root_squash：将root用户及所属组都映射为匿名用户或用户组（默认设置）；
no_root_squash：与rootsquash取反；
anonuid=xxx：将远程访问的所有用户都映射为匿名用户，并指定该用户为本地用户（UID=xxx）；
anongid=xxx：将远程访问的所有用户组都映射为匿名用户组账户，并指定该匿名用户组账户为本地用户组账户（GID=xxx）；
```

其它选项

```
secure：限制客户端只能从小于1024的tcp/ip端口连接nfs服务器（默认设置）；
insecure：允许客户端从大于1024的tcp/ip端口连接服务器；
sync：将数据同步写入内存缓冲区与磁盘中，效率低，但可以保证数据的一致性；
async：将数据先保存在内存缓冲区中，必要时才写入磁盘；
wdelay：检查是否有相关的写操作，如果有则将这些写操作一起执行，这样可以提高效率（默认设置）；
no_wdelay：若有写操作则立即执行，应与sync配合使用；
subtree：若输出目录是一个子目录，则nfs服务器将检查其父目录的权限(默认设置)；
no_subtree：即使输出目录是一个子目录，nfs服务器也不检查其父目录的权限，这样可以提高效率；
```

#### 启动nfs服务

nfs服务可以通过systemd管理。

开机启动nfs: sudo systemctl enable nfs

启动nfs:sudo systemctl start nfs

### NFS客户端

#### 挂载NFS共享盘

```
mkdir -p /mnt/dir
mount -t nfs 192.168.20.204:/home/dir /mnt/dir
```

访问/mnt/dir，查看其中存在内容即挂载成功。

#### 开机自动挂载

文件fstab包含了你的电脑上的存储设备及其文件系统的信息。它是决定一个硬盘（分区）被怎样使用或者说整合到整个系统中的唯一文件。具体来说：用fstab可以自动挂载各种文件系统格式的硬盘、分区、可移动设备和远程设备等。对于Windows与arch双操作系统用户，用fstab挂载FAT格式和NTFS格式的分区，可以在Linux中共享windows系统下的资源。

编辑/etc/fstab文件，添加一行nfs共享盘加载记录即可。格式如：

```
<server>:</remote/export> </local/directory> nfs < options> 0 0
```

示例：

```
192.168.0.96:/home/static /mnt/96static         nfs     defaults        0 0
```


