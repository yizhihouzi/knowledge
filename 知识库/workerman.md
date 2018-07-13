## WorkerMan 使用

> 依赖安装

```
yum install libevent-devel -y
pecl install event
#注意提示：Include libevent OpenSSL support [yes] : 时输入no回车，其它直接敲回车就行
echo extension=event.so > /etc/php.d/event.ini
```