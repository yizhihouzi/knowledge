## Mongodb安装配置

```
YUM安装企业版：https://docs.mongodb.com/master/tutorial/install-mongodb-enterprise-on-red-hat/
YUM安装社区版：https://docs.mongodb.com/master/tutorial/install-mongodb-on-red-hat/
参考文章：http://blog.topspeedsnail.com/archives/6005
```

### 使用yum安装mongodb

* a. 创建MongoDB Linux packages repository

```
vim /etc/yum.repos.d/mongodb-enterprise.repo
```

```
[mongodb-enterprise]
name=MongoDB Enterprise Repository
baseurl=https://repo.mongodb.com/yum/redhat/$releasever/mongodb-enterprise/3.4/$basearch/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc
```

* b.安装

```
sudo yum install -y mongodb-enterprise
```

* c.启动\停止\重启\开机启动 MongoDB

```
systemctl start/restart/stop mongod
```

* d.客户端登陆

```
mongo
```

### 配置mongodb

* a.解决”transparent huge page error”警告信息

```
vim /etc/rc.local
+++
echo never > /sys/kernel/mm/transparent_hugepage/enabled
+++
echo never > /sys/kernel/mm/transparent_hugepage/defrag

systemctl restart mongod.service
```

* b.解决 rlimits 相关的警告

```
vim /etc/security/limits.d/20-nproc.conf
+++
mongod   soft  nproc   64000

systemctl restart mongod.service
```

* c.默认mongodb无需账户登陆,生产环境时需要添加账户验证

```
详见
https://docs.mongodb.com/master/tutorial/enable-authentication/?_ga=1.255148936.1604166735.1481697824
```

