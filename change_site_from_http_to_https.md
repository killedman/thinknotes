---
title: 改造http网站为https
date: 2018-05-12
---

## Let's Encrypt
[Let’s Encrypt](https://letsencrypt.org/) 是一个于2015年推出的数字证书认证机构，将通过旨在消除当前手动创建和安装证书的复杂过程的自动化流程，为安全网站提供免费的SSL/TLS证书。这是由互联网安全研究小组（ISRG – Internet Security Research Group，一个公益组织）提供的服务。主要赞助商包括电子前哨基金会，Mozilla基金会，Akamai以及Cisco等公司（赞助商列表）。如果你的网站有域名可以使用这个方案进行https配置，如果只有公网ip没有域名则没有办法使用这个免费的解决方案。具体配置可以参考[如何免费的让网站启用HTTPS](https://coolshell.cn/articles/18094.html) 

### Let’s Encrypt 的证书90天就过期了

所以，你还要设置上自动化的更新脚本，最容易的莫过于使用 crontab 了。使用 crontab -e 命令加入如下的定时作业（每个月都强制更新一下）：


0 0 1 * * /usr/bin/certbot renew --force-renewal

5 0 1 * * /usr/sbin/service nginx restart


## 自签名证书
> Note: A self-signed certificate will encrypt communication between your server and any clients. However, because it is not signed by any of the trusted certificate authorities included with web browsers, users cannot use the certificate to validate the identity of your server automatically.
> A self-signed certificate may be appropriate if you do not have a domain name associated with your server and for instances where the encrypted web interface is not user-facing. If you do have a domain name, in many cases it is better to use a CA-signed certificate. 
> 注意：自签名证书将加密您的服务器和任何客户端之间的通信。 但是，由于它未被任何Web浏览器附带的可信证书颁发机构签名，因此用户无法使用该证书自动验证服务器的身份。
> 如果您没有与您的服务器关联的域名以及加密的Web界面不面向用户的情况，则自签名证书可能是适当的。 如果您确实有域名，则在许多情况下最好使用CA签名的证书。

## 使用openssl生成自签名证书实现网站https
1. 参考[How To Create a Self-Signed SSL Certificate for Nginx on CentOS 7](https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-nginx-on-centos-7)使用openssl生成证书，在nginx中进行了https的配置，实现了自签名的https配置
2. 网站本身使用flask，使用http方式部署，通过nginx反向代理实现了https访问
3. flask程序在nginx.conf中的配置如下:

<pre>
location / {
    proxy_pass http://127.0.0.1:5000;
    proxy_set_header Host $host;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}
</pre>

## 配置过程和内容

### Create the SSL Certificate 创建SSL证书

#### 创建目录

1. The /etc/ssl/certs directory, which can be used to hold the public certificate, should already exist on the server. Let's create an /etc/ssl/private directory as well, to hold the private key file. Since the secrecy of this key is essential for security, we will lock down the permissions to prevent unauthorized access:
2. mkdir /etc/ssl/private
3. chmod 700 /etc/ssl/private

#### 创建自签名key和证书对

1. create a self-signed key and certificate pair with OpenSSL
<pre>openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/nginx-selfsigned.key -out /etc/ssl/certs/nginx-selfsigned.crt</pre>
2. Both of the files you created will be placed in the appropriate subdirectories of the /etc/ssl directory.

#### 创建Diffie-Hellman

1. While we are using OpenSSL, we should also create a strong Diffie-Hellman group, which is used in negotiating Perfect Forward Secrecy with clients.
<pre>openssl dhparam -out /etc/ssl/certs/dhparam.pem 2048</pre>
This may take a few minutes, but when it's done you will have a strong DH group at /etc/ssl/certs/dhparam.pem that we can use in our configuration.

### nginx.conf主要配置文件

<pre>
server {
    listen 443 http2 ssl;
    listen [::]:443 http2 ssl;

    server_name server_IP_address;

    ssl_certificate /etc/ssl/certs/nginx-selfsigned.crt;
    ssl_certificate_key /etc/ssl/private/nginx-selfsigned.key;
    ssl_dhparam /etc/ssl/certs/dhparam.pem;

    ########################################################################
    # from https://cipherli.st/                                            #
    # and https://raymii.org/s/tutorials/Strong_SSL_Security_On_nginx.html #
    ########################################################################

    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;
    ssl_ciphers "EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH";
    ssl_ecdh_curve secp384r1;
    ssl_session_cache shared:SSL:10m;
    ssl_session_tickets off;
    ssl_stapling on;
    ssl_stapling_verify on;
    resolver 8.8.8.8 8.8.4.4 valid=300s;
    resolver_timeout 5s;
    # Disable preloading HSTS for now.  You can use the commented out header line that includes
    # the "preload" directive if you understand the implications.
    #add_header Strict-Transport-Security "max-age=63072000; includeSubdomains; preload";
    add_header Strict-Transport-Security "max-age=63072000; includeSubdomains";
    add_header X-Frame-Options DENY;
    add_header X-Content-Type-Options nosniff;

    ##################################
    # END https://cipherli.st/ BLOCK #
    ##################################
	
    location / {
    proxy_pass http://127.0.0.1:5000;
    proxy_set_header Host $host;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }

    error_page 404 /404.html;
    location = /404.html {
    }

    error_page 500 502 503 504 /50x.html;
    location = /50x.html {
    }
}
</pre>


## HTTPS的相关知识

1. [阮一峰的[HTTPS的七个误解（译文）](http://www.ruanyifeng.com/blog/2011/02/seven_myths_about_https.html)
