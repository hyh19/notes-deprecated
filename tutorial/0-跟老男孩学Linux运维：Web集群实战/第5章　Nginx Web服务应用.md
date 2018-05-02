[TOC]

# 第5章　Nginx Web服务应用

## 5.1　Nginx介绍

N/A

## 5.2　Nginx Web服务

N/A

## 5.3　编译安装Nginx

参考以下笔记：

http://note.youdao.com/noteshare?id=a3e42a3ca5efee2dff6c05cac0f451b4

## 5.4　Nginx技术的深入剖析

N/A

## 5.5　Nginx虚拟主机配置实战

### 5.5.1　虚拟主机的概念和类型介绍

> 所谓虚拟主机，在Web服务里就是一个独立的网站站点，这个站点对应独立的域名（也可能是IP或端口），具有独立的程序及资源目录，可以独立地对外提供服务供用户访问。

> 常见的虚拟主机类型有如下几种。
> 
> （1）基于域名的虚拟主机
> 
> （2）基于端口的虚拟主机
> 
> （3）基于IP的虚拟主机

### 5.5.2　基于域名的虚拟主机配置实战

#### 示例一：单个基于域名的虚拟主机配置

`www.conf`
```nginx
server {
    listen 80;
    server_name www.etiantian.org;
    location / {
        root /usr/share/nginx/html/www;
        index index.html index.htm;
    }
}
```

```bash
nginx -t
nginx -s reload
```

修改主机域名
```bash
# Linux
echo "159.65.13.118 www.etiantian.org" >> /etc/hosts

# Mac OS X
sudo vim /private/etc/hosts
159.65.13.118 www.etiantian.org
```

```
$ curl www.etiantian.org
www.etiantian.org
```

#### 示例二：多个基于域名的虚拟主机配置

`bbs.conf`
```nginx
server {
    listen 80;
    server_name bbs.etiantian.org;
    location / {
        root /usr/share/nginx/html/bbs;
        index index.html index.htm;
    }
}
```

`blog.conf`
```nginx
server {
    listen 80;
    server_name blog.etiantian.org;
    location / {
        root /usr/share/nginx/html/blog;
        index index.html index.htm;
    }
}
```

```bash
echo "159.65.13.118 bbs.etiantian.org blog.etiantian.org" >> /etc/hosts
```

```
$ curl bbs.etiantian.org
bbs.etiantian.org

$ curl blog.etiantian.org
blog.etiantian.org
```

### 5.5.3　基于端口的虚拟主机配置实战

`www.conf`
```nginx
server {
    listen 80;
    server_name www.etiantian.org;
    location / {
        root /usr/share/nginx/html/www;
        index index.html index.htm;
    }
}
```

`bbs.conf`
```nginx
server {
    listen 81;
    server_name bbs.etiantian.org;
    location / {
        root /usr/share/nginx/html/bbs;
        index index.html index.htm;
    }
}
```

`blog.conf`
```nginx
server {
    listen 82;
    server_name blog.etiantian.org;
    location / {
        root /usr/share/nginx/html/blog;
        index index.html index.htm;
    }
}
```

```
$ curl www.etiantian.org:80 
www.etiantian.org

$ curl bbs.etiantian.org:81
bbs.etiantian.org

$ curl blog.etiantian.org:82
blog.etiantian.org
```

### 5.5.4　基于IP的虚拟主机配置实战

添加虚拟 IP 地址
```
$ ip a add 10.15.0.6/16 dev eth0
$ ip a add 10.15.0.7/16 dev eth0

$ ip a | grep 10.15.0
    inet 10.15.0.5/16 brd 10.15.255.255 scope global eth0
    inet 10.15.0.6/16 scope global secondary eth0
    inet 10.15.0.7/16 scope global secondary eth0
```

`www.conf`
```nginx
server {
    listen 10.15.0.5;
    server_name _;
    location / {
        root /usr/share/nginx/html/www;
        index index.html index.htm;
    }
}
```

`bbs.conf`
```nginx
server {
    listen 10.15.0.6;
    server_name _;
    location / {
        root /usr/share/nginx/html/bbs;
        index index.html index.htm;
    }
}
```

`blog.conf`
```nginx
server {
    listen 10.15.0.7;
    server_name _;
    location / {
        root /usr/share/nginx/html/blog;
        index index.html index.htm;
    }
}
```

```
$ curl 10.15.0.5
www.etiantian.org

$ curl 10.15.0.6
bbs.etiantian.org

$ curl 10.15.0.7
blog.etiantian.org
```

### 5.5.5　Nginx配置虚拟主机的步骤

[How nginx processes a request](http://nginx.org/en/docs/http/request_processing.html)

### 5.5.6　企业场景中重启Nginx后的检测策略

跳过

## 5.6　Nginx常用功能配置实战

### 5.6.1　规范优化Nginx配置文件

N/A

### 5.6.2　Nginx虚拟主机的别名配置

`www.conf`
```nginx
server {
    listen 80;
    server_name www.etiantian.org etiantian.org;
    location / {
        root /usr/share/nginx/html/www;
        index index.html index.htm;
    }
}
```

```bash
echo "159.65.15.233 www.etiantian.org etiantian.org" >> /etc/hosts
```

```
$ curl www.etiantian.org
www.etiantian.org

$ curl etiantian.org
www.etiantian.org
```

### 5.6.3　Nginx状态信息功能实战

[stub_status](http://nginx.org/en/docs/http/ngx_http_stub_status_module.html#stub_status) 

[ngx_http_stub_status_module](http://nginx.org/en/docs/http/ngx_http_stub_status_module.html)

> Nginx软件的功能模块中有一个ngx_http_stub_status_module模块，这个模块的主要功能是记录Nginx的基本访问状态信息，让使用者了解Nginx的工作状态，例如连接数等信息。要使用状态模块，在编译Nginx时必须增加http_stub_status_module模块来支持。

检查 Nginx 的编译参数是否包含模块 `ngx_http_stub_status_module`
```bash
nginx -V | grep http_stub_status_module
```

`status.conf`
```nginx
server {
    listen 80;
    server_name status.etiantian.org;
    location / {
        stub_status on;
        access_log off;
    }
}
```

```bash
echo "159.65.15.233 status.etiantian.org" >> /etc/hosts
```

```bash
$ curl status.etiantian.org         
Active connections: 1 
server accepts handled requests
 28 28 28 
Reading: 0 Writing: 1 Waiting: 0 
```

只允许某些主机查看状态信息

`status.conf`
```nginx
server {
    listen 80;
    server_name status.etiantian.org;
    location / {
        stub_status on;
        access_log off;
        allow 159.65.15.233;
        deny all;
    }
}
```

### 5.6.4　为Nginx增加错误日志（error_log）配置

[error_log](http://nginx.org/en/docs/ngx_core_module.html#error_log)

复习到这

## 5.7　Nginx访问日志（access_log）

### 5.7.1　Nginx访问日志介绍

[ngx_http_log_module](http://nginx.org/en/docs/http/ngx_http_log_module.html) 

### 5.7.2　访问日志参数

[access_log (ngx_http_log_module)](http://nginx.org/en/docs/http/ngx_http_log_module.html#access_log) \
[log_format (ngx_http_log_module)](http://nginx.org/en/docs/http/ngx_http_log_module.html#log_format)

### 5.7.3　访问日志配置说明

N/A

### 5.7.4　访问日志配置实战

`www.conf`
```nginx
server {
    listen 80;
    server_name www.etiantian.org etiantian.org;
    location / {
        root /usr/share/nginx/html/www;
        index index.html index.htm;
    }
    access_log /var/log/nginx/access_www.log main;
}
```

```
$ cat /var/log/nginx/access_www.log
159.65.132.169 - - [15/Feb/2018:14:24:56 +0000] "GET / HTTP/1.1" 200 18 "-" "curl/7.29.0" "-"
85.203.47.58 - - [15/Feb/2018:14:26:59 +0000] "GET / HTTP/1.1" 200 18 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_2) AppleWebKit/602.3.12 (KHTML, like Gecko) Version/10.0.2 Safari/602.3.12" "-"
```

可以在记录日志参数中加上 `buffer` 和 `flush` 选项，这样可在高并发场景下提升网站访问性能。

`www.conf`
```nginx
server {
    listen 80;
    server_name www.etiantian.org etiantian.org;
    location / {
        root /usr/share/nginx/html/www;
        index index.html index.htm;
    }
    access_log /var/log/nginx/access_www.log main gzip buffer=32k flush=5s;
}
```

### 5.7.5　Nginx访问日志轮询切割

> ```
> [root@www logs]# cat /server/script/cut_nginx_log.sh
> #！/bin/sh
> Dateformat=`date +%Y%m%d`
> Basedir="/application/nginx"
> Nginxlogdir="$Basedir/logs"
> Logname="access_www"
> [ -d $Nginxlogdir ] && cd $Nginxlogdir||exit 1
> [ -f ${Logname}.log ]||exit 1
> /bin/mv ${Logname}.log ${Dateformat}_${Logname}.log
> $Basedir/sbin/nginx -s reload
> ```
> 
> ```
> cat >>/var/spool/cron/root << EOF
> #cut nginx access log by oldboy
> 00 00 * * * /bin/sh /server/script/cut_nginx_log.sh >/dev/null 2>&1
> EOF
> ```

## 5.8　Nginx location

[location](http://nginx.org/en/docs/http/ngx_http_core_module.html#location) 


## 5.9　Nginx rewrite

### 5.9.1　什么是Nginx rewrite？

N/A

### 5.9.2　Nginx rewrite语法

N/A

### 5.9.3　Nginx rewrite的企业应用场景

N/A

### 5.9.4　Nginx rewrite 301跳转

[rewrite](http://nginx.org/en/docs/http/ngx_http_rewrite_module.html#rewrite)

`www.conf `
```nginx
server {
    listen 80;
    server_name etiantian.org;
    rewrite ^/(.*) http://www.etiantian.org/$1 permanent;
}

server {
    listen 80;
    server_name www.etiantian.org;
    location / {
        root /usr/share/nginx/html/www;
        index index.html index.htm;
    }
    access_log /var/log/nginx/access_www.log main gzip buffer=32k flush=5s;
}
```

### 5.9.5　实现不同域名的URL跳转

> 例1：实现访问http://blog.etiantian.org时跳转到http://www.etiantian.org/blog/oldboy.html。

`blog.conf`
```nginx
server {
    listen 80;
    server_name blog.etiantian.org;
    location / {
        root /usr/share/nginx/html/blog;
        index index.html index.htm;
    }
    if ($http_host ~* "^(.*)\.etiantian\.org$") {
        set $domain $1;
        rewrite ^(.*) http://www.etiantian.org/$domain/oldboy.html break;
    }
}
```

`www.conf`
```nginx
server {
    listen 80;
    server_name www.etiantian.org etiantian.org;
    location / {
        root /usr/share/nginx/html/www;
        index index.html index.htm;
    }
    access_log /var/log/nginx/access_www.log main gzip buffer=32k flush=5s;
}
```

> 例2：实现访问http://etiantian.org/bbs时跳转到http://bbs.etiantian.org。

`www.conf`
```nginx
server {
    listen 80;
    server_name www.etiantian.org etiantian.org;
    location / {
        root /usr/share/nginx/html/www;
        index index.html index.htm;
    }
    rewrite ^(.*)/bbs/ http://bbs.etiantian.org break;
    access_log /var/log/nginx/access_www.log main gzip buffer=32k flush=5s;
}
```

`bbs.conf`
```nginx
server {
    listen 80;
    server_name bbs.etiantian.org;
    location / {
        root /usr/share/nginx/html/bbs;
        index index.html index.htm;
    }
}
```

## 5.10　Nginx访问认证

`www.conf`
```nginx
server {
    listen 80;
    server_name www.etiantian.org etiantian.org;
    location / {
        root /usr/share/nginx/html/www;
        index index.html index.htm;
        auth_basic "oldboy training";
        auth_basic_user_file /etc/nginx/htpasswd;
    }
    access_log /var/log/nginx/access_www.log main gzip buffer=32k flush=5s;
}
```

设置账号和密码
```bash
yum install httpd -y
htpasswd -bc /etc/nginx/htpasswd oldboy 123456
chmod 400 /etc/nginx/htpasswd
chown nginx /etc/nginx/htpasswd 
```

## 5.11　Nginx相关问题的解答

N/A