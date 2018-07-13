# phpMyAdmin安装配置

```
下载网页：https://www.phpmyadmin.net/downloads/
下载地址:https://files.phpmyadmin.net/phpMyAdmin/4.6.5.2/phpMyAdmin-4.6.5.2-all-languages.zip
```

1) 下载/解压/拷贝至web目录

```
wget https://files.phpmyadmin.net/phpMyAdmin/4.6.5.2/phpMyAdmin-4.6.5.2-all-languages.zip
unzip phpMyAdmin-4.6.5.2-all-languages.zip
mv phpMyAdmin-4.6.5.2-all-languages /path/toweb/phpmyadmin
```

2) 编写配置文件并保存至/path/toweb/phpmyadmin

```
vim config.inc.php
+++
<?php
$i = 1;
$cfg['Servers'][$i]['verbose'] = 'localarvin';
$cfg['Servers'][$i]['host'] = '127.0.0.1';//warning:not to be localhost
$cfg['Servers'][$i]['port'] = '3306';
$cfg['Servers'][$i]['socket'] = '';
$cfg['Servers'][$i]['connect_type'] = 'tcp';
$cfg['Servers'][$i]['auth_type'] = 'http';
$cfg['Servers'][$i]['user'] = 'root';//yourusername
$cfg['Servers'][$i]['password'] = '测试mysql23456';//yourpwd
$cfg['DefaultLang'] = 'en';
$cfg['ServerDefault'] = 1;
$cfg['UploadDir'] = '';
$cfg['SaveDir'] = '';
?>

```

3) 配置nginx虚拟域名(如:phpmyadmin.cn),并将web目录指向/path/toweb/phpmyadmin

```
sudo vim /etc/nginx/conf.d/phpmyadmin.local.conf
+++
server {
    listen       80;
    server_name  phpmyadmin.ceshi;

    #charset koi8-r;
    #access_log  /var/log/nginx/log/host.access.log  main;

    root   /usr/share/nginx/html/phpmyadmin;
    
    location / {
    #注意需要添加index.php  才能直接以域名访问
        index  index.php index.html index.htm;
    }

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    #
    location ~ \.php$ {
    #    root           html;
        fastcgi_pass   127.0.0.1:9000;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
        include        fastcgi_params;
    }

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    #
    #location ~ /\.ht {
    #    deny  all;
    #}
}
```



4) 浏览器访问http://phpmyadmin.cn,输入你的用户名及密码即可
