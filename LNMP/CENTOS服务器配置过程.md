# Linux安装配置  

```
为了避免SELINUX在后续设置中作乱，建议一开始就禁用掉SELINUX。禁用方法：
vi /etc/sysconfig/selinux
注释或者删除以下两行：
SELINUX=enforcing
SELINUXTYPE=targeted
在后面添加一行：
SELINUX=disabled
```

1) 分配管理网站的用户：www 

* a. adduser -U www 

* b. passwd www  

2) 允许www用户使用sudo命令 

* a．vim /etc/sudoers 

* b．www ALL=(ALL) ALL

//在文件末尾添加后保存

* c．su www

//之后的操作都切换到www用户下来操作

3) 修改系统环境变量

      * 临时修改（当前用户、当前终端有效）
      
      export PATH=$PATH:/usr/local/php7/bin

      * 永久有效，但仅对当前用户
      
      ```
      #打开~/.bashrc文件：vim ~/.bashrc
      #在最后一行添加：export PATH=$PATH:/usr/local/php7/bin
      #终端中执行：source ~/.bashrc
      ```

      * 永久有效，对所有用户（需要重启）
      
      ```
      #修改/etc/profile文件：vim /etc/profile
      #找到设置PATH的行：/export PATH
      #修改:export PATH=$PATH:/usr/local/php7/bin
      #重启
      ```

## TIP:  

1) 安装locate命令 

* a. yum install mlocate

* b. updatedb  

2) no acceptable C compiler found in $PATH

* a. sudo yum install gcc  

3) libmcrypt安装

* a. wget http://softlayer.dl.sourceforge.net/sourceforge/mcrypt/libmcrypt-2.5.8.tar.gz

* b. tar -zxvf libmcrypt-2.5.8.tar.gz

* c. cd libmcrypt-2.5.8

* d. sudo ./configure --prefix=/usr/local

* e. sudo make

* f. sudo make install  

4) 安装gcc gcc++

* a. yum install gc gcc++ *gcc-c++*

5) 查看磁盘使用情况

```
df -lh
```

6) linux磁盘分区

```
/boot ：用来存放与 Linux 系统启动有关的程序，比如启动引导装载程序等，建议大小为 500MB 。 
/usr ：用来存放 Linux 系统中的应用程序，其相关数据较多。 
/var ：用来存放 Linux 系统中经常变化的数据以及日志文件。 
/home ：存放普通用户的数据，是普通用户的宿主目录，建议大小为剩下的空间。 
/ ： Linux 系统的根目录，所有的目录都挂在这个目录下面，建议大小为 5GB 以上。 
/tmp ：将临时盘在独立的分区，可避免在文件系统被塞满时影响到系统的稳定性。建议大小为 20GB 以上。 
swap ：实现虚拟内存，建议大小是物理内存的 1~2 倍。

根分区 / 必须总是物理地包含 /etc、/bin、/sbin、/lib 和 /dev，否则您将不能启动系统。
/usr：包含所有的用户程序(/usr/bin)，库文件(/usr/lib)，文档(/usr/share/doc)，等等。这是文件
系统中耗费空间最多的部分。
/var：所有的可变数据，如新闻组文章、电子邮件、网站、数据库、软件包系统的缓存等等，将被放入
这个目录。

```

7) ifconfig和netstat命令安装

```
yum install net-tools
```

8) DNS修改

    * 临时修改
    
    ```
    sudo vim /etc/resolv.conf
    #修改或添加nameserver行
    ```
    
    * 永久修改
    
    ```
    #显示当前网络连接
    nmcli connection show
    #NAME UUID                                 TYPE           DEVICE
    #eno1 5fb06bd0-0bb0-7ffb-45f1-d6edd65f3e03 802-3-ethernet eno1
    #修改当前网络连接对应的DNS服务器，这里的网络连接可以用名称或者UUID来标识
    nmcli con mod eno1 ipv4.dns "223.5.5.5 8.8.8.8"
    #将dns配置生效
    nmcli con up eno1
    ```
    
9) 中文拼音输入法安装

有的时候不小心删除了中文输入法,需要重新安装

```
yum install  ibus-libpinyin
```

上面的安装自行完成后需要注销重新登陆,然后进入设置->区域和语言->输入源添加汉语pinyin输入法

10) KMPLAYER安装

参考连接:

```
http://www.linuxidc.com/Linux/2014-11/108916.htm
#(解决安装kmplayer时可能遇见的问题)
http://www.mamicode.com/info-detail-1195831.html
```

11) GNOME安装

```
sudo  yum groupinstall "GNOME Desktop" "Graphical Administration Tools"
sudo ln -sf /lib/systemd/system/runlevel5.target /etc/systemd/system/default.target
```

