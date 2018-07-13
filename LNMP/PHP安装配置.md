# 编译安装php 

* 1.   下载安装包
  ```bash
  cd ~ && mkdir downloads && cd downloads && wget http://cn2.php.net/distributions/php-7.2.1.tar.gz
  ```
* 2.    解压压缩包并切换目录
  ```bash
  tar -zxvf php-7.2.1.tar.gz && cd php-7.2.1
  ```
* 3.  安装准备相关依赖项
  ```bash
  sudo yum -y install libxml2 libxml2-devel openssl openssl-devel curl-devel libjpeg-devel libpng-devel freetype-devel systemd-devel libmcrypt libmcrypt-devel  
  ```

* 4.   安装前配置
   ```bash
  ;prefix 可根据实际情况自定义
  ;with-fpm-systemd参数可以使PHP7支持centos7的systemd服务管理
  ;安装过程中可能有一些扩展需要额外库支持，如果configure运行报错，直接goolge搜索，基本上yum install xxx-devel 就可
  ./configure --prefix=/usr/local/php7 \
  --with-config-file-path=/usr/local/php7/etc \
  --with-config-file-scan-dir=/usr/local/php7/etc/php.d \
  --enable-mysqlnd \
  --with-mysqli \
  --with-pdo-mysql \
  --enable-fpm \
  --with-fpm-user=nobody \
  --with-fpm-group=nobody \
  --with-fpm-systemd \
  --with-gettext \
  --with-gd \
  --with-jpeg-dir \
  --with-freetype-dir \
  --with-iconv \
  --with-zlib \
  --enable-xml \
  --enable-shmop \
  --enable-sysvsem \
  --enable-inline-optimization \
  --enable-mbregex \
  --enable-mbstring \
  --enable-ftp \
  --with-openssl \
  --enable-pcntl \
  --enable-sockets \
  --with-xmlrpc \
  --enable-zip \
  --enable-soap \
  --with-pear \
  --enable-session \
  --with-curl \
  --enable-opcache
   ```
* 5.  编辑安装
  ```bash
  sudo make && sudo make install
  ```
* 6.  php-fpm systemd 配置
  ```bash
  sudo cp ./sapi/fpm/php-fpm.service /usr/lib/systemd/system/
  ```
* 7. php-fpm 配置
  ```bash
  sudo mv /usr/local/php7/etc/php-fpm.conf.default /usr/local/php7/etc/php-fpm.conf
  sudo mv /usr/local/php7/etc/php-fpm.d/www.conf.default /usr/local/php7/etc/php-fpm.d/www.conf
  ```
* 8.  复制php.ini及修改时区
  ```bash
  sudo cp php.ini-production /usr/local/php7/etc/php.ini
  sudo vim /usr/local/php7/etc/php.ini
  +++
  date.timezone = Asia/Shanghai
  +++
  ```
* 9.  安装扩展

      如inotify、event。
* 10.   开机启动php-fpm
  ```bash
  sudo systemctl enable php-fpm.service
  ```
* 11.   启动php-fpm
  ```bash
  sudo systemctl start php-fpm.service
  ```

# TIP:

1.  no acceptable C compiler found in $PATH

```bash
sudo yum install gcc
```
2.  libmcrypt安装

```
wget http://sourceforge.mirrorservice.org/m/mc/mcrypt/Libmcrypt/2.5.8/libmcrypt-2.5.8.tar.gz
tar -zxvf libmcrypt-2.5.8.tar.gz
cd libmcrypt-2.5.8
sudo ./configure --prefix=/usr/local
sudo make
sudo make install
```

3.  composer安装

* a.安装composer：curl -sS https://getcomposer.org/installer | php

* b.切换中国镜像源：composer config -g repo.packagist composer https://packagist.phpcomposer.com 

* c.composer知识简介

```
1、更新单个库：composer update foo/bar
2、安装某个库：composer require "foo/bar:1.0.0"
3、安装某个库作为项目目录，而不是引入库到vendor目录下：composer create-project doctrine/orm path 2.2.0 
4、升级composer版本：composer self-update
5、使用本地缓存包：composer update foo/bar --prefer-dist
6、产品发布时优化自动加载：composer dump-autoload --optimize  
```

4.  手动编译php扩展执行phpize命令时，提示：Cannot find autoconf    

如：

```
Configuring for:
PHP Api Version: 20041225
Zend Module Api No: 20060613
Zend Extension Api No: 220060519
Cannot find autoconf. Please check your autoconf installation and the $PHP_AUTOCONF environment variable. Then, rerun this script.
```

这代表系统中未安装m4及autoconf工具，安装即可：

```
sudo yum install m4
sudo yum install autoconf
```

5.  添加php命令到系统环境变量

```
;打开~/.bashrc文件
vim ~/.bashrc
;在最后一行添加
;export PATH=$PATH:/usr/local/php7/bin
;终端中执行
source ~/.bashrc
```

6.  手动下载pecl安装包安装

```
pecl install /path/to/package
```

7.  pecl安装失败时，检测主机与pecl.php.net网络是否通畅，以及清除pecl缓存（pecl clear-cache）
8.  让PHP7达到最高性能的几个Tips

```
1. 开启opcache以及opcache file cache
vim /path/to/php.ini
+++
zend_extension=opcache.so
+++
opcache.enable=1
+++
opcache.enable_cli=1

2. 使用HugePage并开启opcache.huge_code_pages
sudo sysctl vm.nr_hugepages=512
vim /path/to/php.ini
+++
 opcache.huge_code_pages=1
```
9.  event 扩展安装

```bash
;安装libevent
sudo yum install libevent-devel -y
;通过pecl安装event扩展
sudo /usr/local/php7/bin/pecl install event
;修改php.ini
vim /usr/local/php7/etc/php.ini
+++
extension=event.so
```


10.  gmagick extension install

```
sudo yum install -y gcc libpng libjpeg libpng-devel libjpeg-devel ghostscript libtiff libtiff-devel freetype freetype-devel
cd ~/Downloads
wget ftp://ftp.graphicsmagick.org/pub/GraphicsMagick/1.3/GraphicsMagick-1.3.22.tar.gz
tar zxvf GraphicsMagick-1.3.22.tar.gz
cd GraphicsMagick-1.3.22
./configure --enable-shared
sudo make
sudo make install
sudo pecl install gmagick
```

```
#php
paragonie/random_compat suggests installing ext-libsodium (Provides a modern crypto API that can be used to generate random bytes.)
imagine/imagine suggests installing ext-gmagick (to use the Gmagick implementation)
imagine/imagine suggests installing ext-imagick (to use the Imagick implementation)
lcobucci/jwt suggests installing mdanter/ecc (Required to use Elliptic Curves based algorithms.)
monolog/monolog suggests installing aws/aws-sdk-php (Allow sending log messages to AWS services like DynamoDB)
monolog/monolog suggests installing doctrine/couchdb (Allow sending log messages to a CouchDB server)
monolog/monolog suggests installing ext-amqp (Allow sending log messages to an AMQP server (1.0+ required))
monolog/monolog suggests installing ext-mongo (Allow sending log messages to a MongoDB server)
monolog/monolog suggests installing graylog2/gelf-php (Allow sending log messages to a GrayLog2 server)
monolog/monolog suggests installing php-amqplib/php-amqplib (Allow sending log messages to an AMQP server using php-amqplib)
monolog/monolog suggests installing php-console/php-console (Allow sending log messages to Google Chrome)
monolog/monolog suggests installing rollbar/rollbar (Allow sending log messages to Rollbar)
monolog/monolog suggests installing ruflin/elastica (Allow sending log messages to an Elastic Search server)
monolog/monolog suggests installing sentry/sentry (Allow sending log messages to a Sentry server)
respect/validation suggests installing egulias/email-validator (Strict (RFC compliant) email validation)
respect/validation suggests installing ext-bcmath (Arbitrary Precision Mathematics)
respect/validation suggests installing fabpot/php-cs-fixer (Fix PSR2 and other coding style issues)
respect/validation suggests installing malkusch/bav (German bank account validation)
respect/validation suggests installing symfony/validator (Use Symfony validator through Respect\Validation)
respect/validation suggests installing zendframework/zend-validator (Use Zend Framework validator through Respect\Validation)
```




