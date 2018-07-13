## HTTPS配置

Let’s Encrypt 是一个于2015年三季度推出的数字证书认证机构，将通过旨在消除当前手动创建和安装证书的复杂过程的自动化流程，为安全网站提供免费的SSL/TLS证书。Let’s Encrypt 是由互联网安全研究小组（ISRG，一个公益组织）提供的服务。主要赞助商包括电子前哨基金会，Mozilla 基金会，Akamai 以及思科。

官网：https://certbot.eff.org/

> certbot是免费的证书发放机构，为了保证公平性，它会限制域名申请证书的次数。具体规则如下：
>
> 1. 每个注册域名每周可申请20次证书；
> 2. 一次可以为一组子域名申请证书，一组中最多包含100个子域名，同一组域名（与大小写、顺序无关）每周可申请5次；
> 3. 对于一组域名，改变其中一个域名，或增减一个域名，即成为一个新组。

1. 打开 https://certbot.eff.org/ 选择对应操作系统与 Web 服务器，选完后出现响应的平台说明.如CentOS、Nginx的安装说明：https://certbot.eff.org/lets-encrypt/centosrhel7-nginx.html
2. 安装certbot

```bash
sudo yum -y install yum-utils
sudo yum-config-manager --enable rhui-REGION-rhel-server-extras rhui-REGION-rhel-server-optional
sudo yum install python2-certbot-nginx
```

3. 使用certbot程序安装证书

由于certbot dns-plugins不支持阿里云API，故这一步需要通过交互式申请，从而导致后续不能使用crontab配置证书自动续签。

```
certbot-auto certonly --manual -d *.example.com -d example.com --agree-tos --no-bootstrap --manual-public-ip-logging-ok --preferred-challenges dns-01 --server https://acme-v02.api.letsencrypt.org/directory
```
> --manual交互式获取
> --preferred-challenges dns使用DNS验证的方式（泛域名只能使用DNS验证）
> --server指明支持acme-v02的Server地址，默认是acme-v01的地址。

当看到下面信息：
```
------------------------------------------------------------
Please deploy a DNS TXT record under the name
_acme-challenge.amsimple.com with the following value:

JHkwGFgXq3OgedI-4RU1X0EcFUz7cxIPN7r1Qyw5JTw

Before continuing, verify the record is deployed.
------------------------------------------------------------
Press Enter to Continue
```
暂停操作，到阿里云控制台配置域名解析，添加一条TXT类型的解析，等待3-5分钟，使用以下命令检查解析是否设置成功：
```
dig -t txt _acme-challenge.amsimple.com @8.8.8.8
```
域名解析结果正确，再回到certbot申请的流程中敲回车，成功后，certbot会提示证书和私钥的路径，接着在nginx中配置并重启即可，Good luck have fun～

4. 配置nginx

将原有监听的80端口修改为443端口，并增加证书配置

```
#listen       80;
listen 443 ssl;
server_name example.domain www.example.domain;

ssl_certificate /etc/letsencrypt/live/example.domain/fullchain.pem;
ssl_certificate_key /etc/letsencrypt/live/example.domain/privkey.pem;

```

配置80端口转发至443端口

```
server {
    listen 80;
    server_name example.domain www.example.domain;
    return 301 https://$host$request_uri;
}
```

重启nginx

```bash
sudo systemctl restart nginx
```

5. 证书自动更新

由于certbot dns-plugins不支持阿里云API，故这一步需要通过交互式申请，从而导致后续不能使用crontab配置证书自动续签。