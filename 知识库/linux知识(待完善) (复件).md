## Linux知识


```
 
```

sudo chown -R runner2:ipsky static/

//只修改子目录的权限
sudo chmod 0305 `find static/ -type d`

//只修改文件的权限
find static/ -type f -exec chmod 0204 {} \;

### TIP

1) scp使用

```
http://www.cnblogs.com/hitwtx/archive/2011/11/16/2251254.html
    scp是有Security的文件copy，基于ssh登录。操作起来比较方便，比如要把当前一个文件copy到远程另外一台主机上，可以如下命令。

scp /home/daisy/full.tar.gz root@172.19.2.75:/home/root

然后会提示你输入另外那台172.19.2.75主机的root用户的登录密码，接着就开始copy了。

    如果想反过来操作，把文件从远程主机copy到当前系统，也很简单。

linux之cp/scp命令＋scp命令详解(转) - linmaogan - 独木★不成林scp root@/full.tar.gz 172.19.2.75:/home/root/full.tar.gz home/daisy/full.tar.gz

linux 的 scp 命令 可以 在 linux 之间复制 文件 和 目录； 

================== 
scp 命令 
================== 
scp 可以在 2个 linux 主机间复制文件； 

命令基本格式： 
       scp [可选参数] file_source file_target 

====== 
从 本地 复制到 远程 
====== 
* 复制文件： 
        * 命令格式： 
                scp local_file remote_username@remote_ip:remote_folder 
                或者 
                scp local_file remote_username@remote_ip:remote_file 
                或者 
                scp local_file remote_ip:remote_folder 
                或者 
                scp local_file remote_ip:remote_file 

                第1,2个指定了用户名，命令执行后需要再输入密码，第1个仅指定了远程的目录，文件名字不变，第2个指定了文件名； 
                第3,4个没有指定用户名，命令执行后需要输入用户名和密码，第3个仅指定了远程的目录，文件名字不变，第4个指定了文件名； 
        * 例子： 
                scp /home/space/music/1.mp3 root@www.cumt.edu.cn:/home/root/others/music 
                scp /home/space/music/1.mp3 root@www.cumt.edu.cn:/home/root/others/music/001.mp3 
                scp /home/space/music/1.mp3 www.cumt.edu.cn:/home/root/others/music 
                scp /home/space/music/1.mp3 www.cumt.edu.cn:/home/root/others/music/001.mp3 

* 复制目录： 
        * 命令格式： 
                scp -r local_folder remote_username@remote_ip:remote_folder 
                或者 
                scp -r local_folder remote_ip:remote_folder 

                第1个指定了用户名，命令执行后需要再输入密码； 
                第2个没有指定用户名，命令执行后需要输入用户名和密码； 
        * 例子： 
                scp -r /home/space/music/ root@www.cumt.edu.cn:/home/root/others/ 
                scp -r /home/space/music/ www.cumt.edu.cn:/home/root/others/ 

                上面 命令 将 本地 music 目录 复制 到 远程 others 目录下，即复制后有 远程 有 ../others/music/ 目录 


====== 
从 远程 复制到 本地 
====== 
从 远程 复制到 本地，只要将 从 本地 复制到 远程 的命令 的 后2个参数 调换顺序 即可； 

例如： 
        scp root@www.cumt.edu.cn:/home/root/others/music /home/space/music/1.mp3 
        scp -r www.cumt.edu.cn:/home/root/others/ /home/space/music/

最简单的应用如下 : 

scp 本地用户名 @IP 地址 : 文件名 1 远程用户名 @IP 地址 : 文件名 2 

[ 本地用户名 @IP 地址 :] 可以不输入 , 可能需要输入远程用户名所对应的密码 . 

可能有用的几个参数 : 

-v 和大多数 linux 命令中的 -v 意思一样 , 用来显示进度 . 可以用来查看连接 , 认证 , 或是配置错误 . 

-C 使能压缩选项 . 

-P 选择端口 . 注意 -p 已经被 rcp 使用 . 

-4 强行使用 IPV4 地址 . 

-6 强行使用 IPV6 地址 .

 

注意两点：
1.如果远程服务器防火墙有特殊限制，scp便要走特殊端口，具体用什么端口视情况而定，命令格式如下：
#scp -p 4588 remote@www.abc.com:/usr/local/sin.sh /home/administrator
2.使用scp要注意所使用的用户是否具有可读取远程服务器相应文件的权限。

```
