### Chrome安装

```
参考文章：http://blog.csdn.net/johnnyhu90/article/details/42127521
```

1） 操作步骤

```bash
cd ~/downloads
wget http://chrome.richardlloyd.org.uk/install_chrome.sh
chmod u+x install_chrome.sh
./install_chrome.sh
```

在执行安装脚本时可以指定安装稳定版本 "./install_chrome.sh -s"。

2) 启动

启动时在命令行输入 "google-chrome" 即可。不要以root用户启动。

3） 更新

执行 "yum update google-chrome-stable" 或者 "./install_chrome.sh" 即可完成更新工作。

4） 卸载

执行 "yum remove google-chrome-stable " 或者 "./install_chrome.sh -u"。