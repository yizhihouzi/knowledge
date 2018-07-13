# Redis安装配置

```
下载链接地址：http://download.redis.io/releases/redis-3.2.6.tar.gz
相关网页：https://redis.io/download
```

* a.安装执行命令

```
wget http://download.redis.io/releases/redis-3.2.6.tar.gz
tar xzf redis-3.2.6.tar.gz
cd redis-3.2.6
make
sudo make install
```

执行完上述命令，即表示redis已安装完毕。编译好的二进制文件在当前目录的src目录内，包括服务端与客户端两个程序。

* b.启动redis服务

```
src/redis-server
```

* c.启动redis客户端

```
src/redis-cli
```

* d.redis客户端启动后，测试命令

```
示例：
redis> set foo bar
OK
redis> get foo
"bar"
```

* e.添加到系統Systemd服務

sudo vim /usr/lib/systemd/system/redis.service

```
[Unit]
Description=Redis
After=syslog.target network.target remote-fs.target nss-lookup.target

[Service]
//Type=forking
//PIDFile=/var/run/redis.pid
ExecStart=/path/to/redis-server /path/to/redis.conf
ExecReload=/bin/kill -s HUP $MAINPID
ExecStop=/bin/kill -s QUIT $MAINPID
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

編輯完成以後,需要执行systemctl daemon-reload使配置生效。

### 需要补充redis需要进行的安全性配置，如避免默认端口，只向127.0.0.1端口开放等

### 其余配置，参考链接

```
https://linux.cn/article-6719-1.html
```