### Phpiredis

相关页面：https://github.com/nrk/phpiredis

Phpiredis是一个PHP的C扩展，它基于hiredis为PHP提供了一个简单高效的Redis操作，它拥有一个非常快的增量协议解析器。

* 安装

安装Phpiredis前应该先安装hiredis程序，否则安装会失败。

```
git clone https://github.com/nrk/phpiredis.git
cd phpiredis
phpize && ./configure --enable-phpiredis
make && sudo make install
```

以上执行成功后需要将extension=phpiredis.so添加至php.ini文件中

