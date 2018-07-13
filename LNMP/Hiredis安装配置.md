# Hiredis安装配置

Hiredis是redis数据库一个轻量的C语言客户端库。

之所以轻量是由于它只是简单的提供了对redis操作语句支持的接口，并没有实现具体的操作语句的功能。但正是由于这种设计使我们只要熟悉了通用的redis操作语句就可以很容易的使用该库和redis数据库进行交互。

除了支持发送命令和接收应答/应答数据，它提供了对应答数据的解析操作。而且这个基于I/O层的数据流解析操作设计考虑到了复用性，可以对应答数据进行通用的解析操作。

Hiredis仅仅支持二进制安全的redis协议，所以你只能针对版本号大于等于1.2.0的redis服务端使用。

Hiredis库包含多种API，包括同步命令操作API、异步命令操作API和对应答数据进行解析的API。

```
下载链接地址：https://github.com/redis/hiredis/archive/v0.13.3.tar.gz
```

* a.安装执行命令

```
wget https://github.com/redis/hiredis/archive/v0.13.3.tar.gz
tar xzf  v0.13.3.tar.gz
cd hiredis-0.13.3/
make
make install
```