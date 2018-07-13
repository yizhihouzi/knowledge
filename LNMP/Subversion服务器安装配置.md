### Subversion安装配置

1) svn可通过yum安装

```
yum install subversion
```

2) svn仓库目录设定

默认svn仓库在/var/svn/目录，一般会将svn目录放在/home/svn目录中。
下面以创建一个名为repo的仓库为例：

```
mkdir /home/svn
svnadmin create /home/svn/repo
``` 

创建好一个svn仓库目录以后，需要修改svnserve的默认路径：

```
vim /etc/sysconfig/svnserve

---
OPTIONS="-d -r /var/svn"
+++
OPTIONS="-d -r /home/svn"
```

3) svn用户、权限管理

用户管理在/home/svn/repo/conf/passwd文件中，
用户访问策略在/home/svn/repo/conf/authz文件中，
svnserve配置文件在/home/svn/repo/conf/svnserve.conf文件中。

```
#passwd文件：
#[users]节下，每添加一行即可添加一个用户，格式如：
[users]
arvin = mypasswd
```

```
#authz文件：
#[groups]节下为用户分组，格式如：
[groups]
group1 = member1,member2,member3

#可自己根据仓库及仓库下的目录定义分节，如repo仓库：
#[repo:/]代表repo仓库根目录
#值r代表读权限，w代表写权限
# * = 表示除了上面设置了权限的用户组之外，其他任何人都被禁止访问本目录。这个很重要，一定要加上！
[repo:/]
@group1 = r
member4 = rw
* =

#[repo:/dir1]代表repo仓库根目录下dir1目录
[repo:/dir1]
@group1 = rw
* =
```

```
#svnserve.conf文件
#修改最终文件内容如下：
[general]
anon-access = none
auth-access = write
password-db = /home/svn/project/conf/passwd
authz-db = /home/svn/project/conf/authz
```

4) 启动svnserve

```
systemctl start svnserve.service
```

5) 设置开机启动

```
systemctl enable svnserve.service
```

6) 验证svn服务可访问

任意目录checkout仓库，检查是否成功，如：

```
cd ~
svn co svn://127.0.0.1/repo
```

检查以上命令是否可以checkout出仓库内容。

### TIP

1) 通过ps -ef |grep svnserve或者netstat -lntp |grep 3690 
查看svnserve已经启动，可是checkout失败

检查selinux是否开启，如果是，就关闭selinux。
关闭方法见centos系统安装配置文件。