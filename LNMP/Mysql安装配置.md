# 安装MySQL(MariaDB)

```sh
sudo yum install mariadb-server
sudo systemctl start mariadb.service
#设置开机启动
sudo systemctl enable mariadb.service
#这个脚本会经过一些列的交互问答来进行MariaDB的安全设置,如设置密码等
sudo /usr/bin/mysql_secure_installation
#登陆mysql
mysql -uroot -p
```

# TIP: 

1) 主从配置