# 安装NGINX

* 1.   添加nginx安装源
  ```Bash
  sudo vim /etc/yum.repos.d/Nginx.repo
  +++
  [nginx]
  name=nginx repo
  baseurl=http://nginx.org/packages/centos/7/$basearch/
  gpgcheck=0
  enabled=1
  ```
* 2.  yum安装nginx

  ```
  sudo yum install nginx
  ```

* 3.  设置开机启动
  ```bash
  sudo systemctl enable nginx.service
  ```
* 4.  启动nginx
  ```bash
  sudo systemctl start nginx.service
  ```
* 5.  测试安装是否成功
  ```bash
  ;成功下载index.html，即代表安装成功
  wget localhost
  ```
* 6. 防火墙开放80、443端口（防火墙开启时才需要执行）
  ```bash
  sudo firewall-cmd --zone=public --add-port=80/tcp --permanent
  sudo firewall-cmd --zone=public --add-port=443/tcp --permanent
  sudo systemctl restart firewalld.service
  ```