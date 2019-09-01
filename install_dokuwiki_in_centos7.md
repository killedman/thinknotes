---
title: install dokuwiki + nginx in CentOS7 
date: 2019-09-01
tag: [2019年, dokuwiki, 知识库]
category: 技术笔记
---


## 查看已经安装好的Dokuwiki



[thinknotes的wiki](http://wiki.thinknotes.cn)



## 配置Dokuwiki

在Dokuwiki[官网](https://download.dokuwiki.org/ ) 上找到下载的直接链接https://download.dokuwiki.org/src/dokuwiki/dokuwiki-stable.tgz

执行wget https://download.dokuwiki.org/src/dokuwiki/dokuwiki-stable.tgz 下载安装包
执行tar -xzvf dokuwiki-stable.tgz -C dokuwiki 解压到当前目录下，新目录名称修改为dokuwiki
执行chown -R   nginx:nginx   / dokuwiki修改dokuwiki目录权限

添加dokuwiki的nginx配置：

touch /etc/nginx/conf.d/dokuwiki.conf

```shell
server {
    listen 8080;
    server_name wiki.thinknotes.com;## 这里需要修改成自己的域名
    # Maximum file upload size is 4MB - change accordingly if needed
    client_max_body_size 4M;
    client_body_buffer_size 128k;

    root /data/dokuwiki;## 这里填真正的wiki目录
    index doku.php index.php;

    #Remember to comment the below out when you're installing, and uncomment it when done.
    location ~ /(data/|conf/|bin/|inc/) { deny all; }

#   Uncomment this prevents images being displayed !
#    location ~* \.(js|css|png|jpg|jpeg|gif|ico)$ {
#        expires 31536000s;
#        add_header Pragma "public";
#        add_header Cache-Control "max-age=31536000, public, must-revalidate, proxy-revalidate";
#        log_not_found off;
#    }

    location / { try_files $uri $uri/ @dokuwiki; }

    location @dokuwiki {
        # rewrites for userewrite=1
        rewrite ^/_media/(.*) /lib/exe/fetch.php?media=$1 last;
        rewrite ^/_detail/(.*) /lib/exe/detail.php?media=$1 last;
        rewrite ^/_export/([^/]+)/(.*) /doku.php?do=export_$1&id=$2 last;
        rewrite ^/(.*) /doku.php?id=$1&$args last;
    }

    location ~ \.php$ {
        #try_files $uri $uri/ /doku.php;
        fastcgi_pass   127.0.0.1:9000;
        fastcgi_index  index.php;
        #fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;  #指定路径
        include        fastcgi_params;
    }
}

```





## 安装php、php-fpm

rpm -qa|grep php 确认php版本，如果版本较低可以使用yum   remove   php*   卸载老版本的php


添加yum源

```shell
rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm
```

安装php和php-fpm

```shell
yum install php70w -y
yum install php70w-fpm -y
systemctl enable php-fpm.service
```

执行vim  /etc/php-fpm.d/www.conf命令修改启动php-fpm的用户和组为nginx
修改user = apache 为user = nginx
修改group = apache 为 group = nginx
保存退出，重启服务：   service     php-fpm restart
此时ps -ef|grep php-fpm 可以看到启动的用户以及从apache变成了nginx

重启下nginx : service    nginx   restart



## 二级域名注册

在域名注册商管理页面添加二级域名，比如wiki.thinknotes.cn

添加成功后等待几分钟，在浏览器中输入wiki.thinknotes.cn/install.php  就可以看到ddokuwiki的配置页面了，如果弹出窗口提示: you have chosen to open a file，说明此时的nginx和php的配置没有做好，或者浏览器有缓存，可以尝试关闭浏览器后再次尝试。



## 初始化dokuwiki

使用用http://域名/install.php 配置完wiki后，修改nginx中dokuwiki.conf，将install.php添加到deny中，不允许其他人访问:



deny前的配置：

```shell
location ~ /(data/|conf/|bin/|inc/) { deny all; }
```

deny后的配置：



```shell
location ~ /(data/|conf/|bin/|inc/|install.php) { deny all; }
```

## 参考

http://blog.gavinzh.com/2017/08/12/Nginx-DokuWiki-PHP-build-wiki/
https://www.dokuwiki.org/zh:manual
https://blog.csdn.net/mergerly/article/details/79629562


