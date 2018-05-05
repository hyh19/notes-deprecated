[TOC]

# 第6章　企业级LNMP环境应用实践

## 6.1　LNMP应用环境

### 6.1.1　LNMP介绍

N/A

### 6.1.2　LNMP组合工作流程

> 当LNMP组合工作时，首先是用户通过浏览器输入域名请求Nginx Web服务，如果请求是静态资源，则由Nginx解析返回给用户；如果是动态请求（.php结尾），那么Nginx就会把它通过FastCGI接口（生产常用方法）发送给PHP引擎服务（FastCGI进程php-fpm）进行解析，如果这个动态请求要读取数据库数据，那么PHP就会继续向后请求MySQL数据库，以读取需要的数据，并最终通过Nginx服务把获取的数据返回给用户，这就是LNMP环境的基本请求顺序流程。

## 6.2　LNMP之MySQL数据库

跳过

## 6.3　FastCGI介绍

### 6.3.1　什么是CGI

N/A

### 6.3.2　什么是FastCGI

N/A

### 6.3.3　Nginx FastCGI的运行原理

N/A

## 6.4　LNMP之PHP（FastCGI方式）服务的安装准备

```bash
yum install -y epel-release
yum install -y zlib-devel libxml2-devel libjpeg-turbo-devel freetype-devel libpng-devel gd-devel libcurl-devel libxslt-devel libmcrypt-devel mhash mcrypt openssl-devel

yum install -y wget
wget https://forensics.cert.org/cert-forensics-tools-release-el7.rpm
rpm -Uvh cert-forensics-tools-release*rpm
yum --enablerepo=forensics install -y libiconv-devel

yum group install -y 'Development Tools'
```

## 6.5　开始安装PHP（FastCGI方式）服务

参考以下笔记：

http://note.youdao.com/noteshare?id=231218546e1abfe1835eb1decae7eb2a

```bash
./configure \
--prefix=/usr/local/php/php-5.6.34 \
--with-mysql \
--with-iconv-dir \
--with-freetype-dir \
--with-jpeg-dir \
--with-png-dir \
--with-zlib \
--with-libxml-dir \
--with-xsl \
--with-mcrypt \
--with-gd \
--with-mhash \
--with-xmlrpc \
--with-openssl \
--with-curl \
--with-fpm-user=nginx \
--with-fpm-group=nginx \
--enable-xml \
--enable-bcmath \
--enable-shmop \
--enable-sysvsem \
--enable-inline-optimization \
--enable-mbregex \
--enable-fpm \
--enable-mbstring \
--enable-gd-native-ttf \
--enable-pcntl \
--enable-sockets \
--enable-zip \
--enable-soap \
--enable-short-tags \
--enable-static \
--enable-ftp \
--disable-rpath
```

## 6.6　配置Nginx支持PHP程序请求访问

[Nginx 1.4.x on Unix systems](http://php.net/manual/en/install.unix.nginx.php)

[How To Install Linux, Nginx, MySQL, PHP (LEMP) stack On CentOS 7](https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-on-centos-7)

**安装**
```bash
yum install -y epel-release
yum install -y nginx
yum install -y php php-mysql php-fpm
```

**配置**

`/etc/php.ini`
```
cgi.fix_pathinfo=0
```

`/etc/php-fpm.d/www.conf`
```
user = nginx
group = nginx
```

`/etc/nginx/conf.d/www.conf`
```nginx
server {
    listen 80;
    server_name www.etiantian.org;
    # 在 server 区块设置，不然，$document_root$fastcgi_script_name 会找不到文件。
    root /usr/share/nginx/html/www/;

    location / {
        index index.php index.html index.htm;
    }

    location ~* \.php$ {
        fastcgi_index   index.php;
        fastcgi_pass    127.0.0.1:9000;
        include         fastcgi_params;
        fastcgi_param   SCRIPT_FILENAME    $document_root$fastcgi_script_name;
        fastcgi_param   SCRIPT_NAME        $fastcgi_script_name;
    }
}
```

**启动**
```bash
nginx
php-fpm
```

**测试**

`/usr/share/nginx/html/www/index.php`
```php
<?php
phpinfo();
?>
```

## 6.7　部署一个blog程序服务

跳过

## 6.8　有关使用高版本PHP 5.5的说明

跳过