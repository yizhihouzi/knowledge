### GitLab项目管理工具

* GitLab 是一个用于仓库管理系统的开源项目。使用Git作为代码管理工具，并在此基础上搭建起来的web服务。Github是公共的git仓库，而Gitlab适合于搭建企业内部私有git仓库。

* 官网安装链接：https://about.gitlab.com/downloads/#centos7

* 参考链接：http://www.tuicool.com/articles/bYbi2mJ

* 清华大学镜像：https://mirror.tuna.tsinghua.edu.cn/help/gitlab-ce/

* 安装教程：http://blog.csdn.net/remote_roamer/article/details/49377465

* GitLab的作用类似于Redmine。

## GitLab 安装总结

* a.确保关闭nginx，php-fpm

* b.配置GitLab依赖的工具

```
sudo yum install curl openssh-server
sudo systemctl enable sshd
sudo systemctl start sshd
sudo yum install postfix
sudo systemctl enable postfix
sudo systemctl start postfix
sudo firewall-cmd --permanent --add-service=http
sudo systemctl reload firewalld
```

* c.安装GitLab package server

1) 配置rpm源

      sudo vim /etc/yum.repos.d/gitlab-ce.repo

```
[gitlab-ce]
name=gitlab-ce
baseurl=http://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/yum/el7
repo_gpgcheck=0
gpgcheck=0
enabled=1
gpgkey=https://packages.gitlab.com/gpg.key
```

2) 更新cache

```
sudo yum makecache
sudo yum install gitlab-ce
```

* d.GitLab设置当前域名及监听端口

```
vim /etc/gitlab/gitlab.rb

#nginx['listen_port'] = 81 
#external_url 'http://monkeystar'
```

* e.配置启动GitLab

```
//这个配置命令可随时运行，不会对当前已存在数据造成影响。
sudo gitlab-ctl reconfigure
```

* f.打开浏览器，输入ip地址即可进入。进入后首先需要设置密码。密码对应账号为root

###TIP

* a.创建备份

```
gitlab-rake gitlab:backup:create
```

使用以上命令会在/var/opt/gitlab/backups目录下创建一个名称类似为1393513186_gitlab_backup.tar的压缩包, 这个压缩包就是Gitlab整个的完整部分, 其中开头的1393513186是备份创建的日期.

* b.修改备份文件默认目录

```
sudo vim /etc/gitlab/gitlab.rb
#gitlab_rails['backup_path'] = '/mnt/backups'
sudo gitlab-ctl reconfigure
```

* c.恢复备份

```
# 停止相关数据连接服务
gitlab-ctl stop unicorn
gitlab-ctl stop sidekiq
# 从1393513186编号备份中恢复
gitlab-rake gitlab:backup:restore BACKUP=1393513186
# 启动Gitlab
sudo gitlab-ctl start
```

* e.迁移gitlab数据

迁移如同备份与恢复的步骤一样, 只需要将老服务器/var/opt/gitlab/backups目录下的备份文件拷贝到新服务器上的/var/opt/gitlab/backups即可(如果你没修改过默认备份目录的话). 但是需要注意的是新服务器上的Gitlab的版本必须与创建备份时的Gitlab版本号相同. 比如新服务器安装的是最新的7.60版本的Gitlab, 那么迁移之前, 最好将老服务器的Gitlab 升级为7.60在进行备份.

* f.gitlab权限管理

GitLib有五种身份权限，分别是：

> Owner 项目所有者，拥有所有的操作权限
> Master 项目的管理者，除更改、删除项目元信息外其它操作均可
> Developer 项目的开发人员，做一些开发工作，对受保护内容无权限
> Reporter 项目的报告者，只有项目的读权限，可以创建代码片断
> Guest 项目的游客，只能提交问题和评论内容