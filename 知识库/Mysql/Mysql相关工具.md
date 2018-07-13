### Mysql相关工具使用

1. GUI客户端

Mysql Workbench 是mysql的官方客户端，功能丰富

PhpMyadmin是使用php实现的mysql web客户端

2. 数据库结构比较工具

mysqldiff是属于MySQL Utilities的一个子工具，可以实现数据库间、数据表间 结构的比较

例：

```mysql
mysqldiff --server1=user1:pwd1@domain1:port --server2=user2:pwd2@domain2:port db1.table1:db2.table2  --skip-table-options
```
> --skip-table-options：这个选项的意思是保持表的选项不变，即对比的差异里面不包括表名、AUTO_INCREMENT,ENGINE, CHARSET等差异。
3. mysql版本升级时修复表结构
> 升级了 MySQL 的软件包，管理数据库的某些表结构发生了变化，所以还需要升级数据库的相关表结构。
```bash
sudo mysql_upgrade --force -uroot -p
```
> 执行mysql_upgrade时要先启动mysql，执行完毕后应该重启mysql