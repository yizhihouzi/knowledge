# 安装Apach ab压测工具

1) ab运行需要依赖apr-util包，安装命令为：

```
yum install apr-util
```

2) 安装依赖 yum-utils中的yumdownload 工具

```
yum install yum-utils
```

3) 下载并安装ab

```
cd ~ && mkdir abtmp
cd abtmp
yum install yum-utils.noarch
yumdownloader httpd-tools*
rpm2cpio httpd-tools*.rpm | cpio -idmv
```

以上操作完成后，abtmp目录中将会产生一个 usr 目录，ab程序文件就在这个usr目录中

### TIP

1) ab命令使用

```
./ab -k  -c 100  -n 1000000 http://www.baidu.com/
#-c用来设定并发请求的数量，此处为100
#-n用来设定请求总数，此处为100万
#-k表示请求时使用长连接，如果不要长连接，就不要-k
#最后接请求的url地址，注意地址后的斜杠不能去掉
```