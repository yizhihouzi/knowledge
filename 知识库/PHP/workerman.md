## Workerman使用

```
官网地址: http://doc3.workerman.net/
```

1) 环境要求

WorkerMan必须运行在unix(Linux & MacOS)环境;

php版本不低于5.4,且安装pcntl、posix扩展

建议安装event或者libevent扩展，但不是必须的(未安装时使用select方式进行事件管理)

检测方法:

```
curl -Ss http://www.workerman.net/check.php | php
#不包含event的检查
```

2) event安装

```
#php的event扩展依赖于libevent-devel,因此需要首先安装libevent-devel
yum install libevent-devel -y

#安装event扩展
#注意提示：Include libevent OpenSSL support [yes] : 时输入no回车，其它直接敲回车就行
pecl install event

#添加event安装配置信息至php.ini
vim /path/to/php.ini
+++
[event]
extension=event.so
```

3) 启动与停止

```
#以debug（调试）方式启动
php start.php start

#以daemon（守护进程）方式启动
php start.php start -d

#1、以debug方式启动，代码中echo、var_dump、print等打印函数会直接输出在终端。
#2、以daemon方式启动，代码中echo、var_dump、print等打印会默认重定向到/dev/null文件，可以通过设置Worker::$stdoutFile = '/your/path/file';来设置这个文件路径。

#停止
php start.php stop

#重启
php start.php restart

#平滑重启
php start.php reload

#查看状态
php start.php status
```



### TIP
1) 不可以通过设置set_exception_handler捕获全局异常,
   但是可以通过设置register_shutdown_function在未捕获异常或错误发生时,执行一些必要的操作.
