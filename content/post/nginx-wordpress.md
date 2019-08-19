---
title: "Nginx配置Wordpress"
date: "2018-01-18"
categories: [中间件]
tags: [
    "Nginx",
]
---

```shell

server {
    listen       8089;
    root         /var/www/wp/;
    index index.html index.php;

        location / {
            #rewrite ^/(.*)$ /index.php?__path__=/$1 last;
            try_files $uri $uri/ /index.php?$args;
        }
        rewrite /wp-admin$ $scheme://$host$uri/ permanent;


        location ~ [^/]\.php(/|$) {
            # comment try_files $uri =404; to enable pathinfo
            try_files $uri =404;
            fastcgi_pass unix:/run/php/php7.1-fpm.sock;
            fastcgi_index index.php;
            include fastcgi.conf;
        }

       location ~* \.(js|css|png|jpg|jpeg|gif|ico)$ {
                log_not_found off;
        }


}

```

### 要点笔记
`/var/www/wp/`则是`wordpress`的安装目录，注意读写权限
`fastcgi_pass`在`PHP`的新版本中要用`unix:/run/php/php7.1-fpm.sock;`的形式

### 踩过的坑
部署wordpress中文版，在界面上初始化后，所有的Web资源(js,css,image)都无法正常加载，安装英文版解决。

