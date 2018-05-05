[toc]

# 第8章　企业级Nginx Web服务优化实战

## 8.1　Nginx基本安全优化

### 8.1.1　调整参数隐藏Nginx软件版本号信息

```bash
$ curl -I www.etiantian.org
HTTP/1.1 200 OK
Server: nginx/1.12.2 # 有显示版本号
Date: Tue, 03 Apr 2018 12:16:29 GMT
Content-Type: text/html
Content-Length: 12
Last-Modified: Tue, 03 Apr 2018 12:03:34 GMT
Connection: keep-alive
ETag: "5ac36d96-c"
Accept-Ranges: bytes
```

加到http区块内，对所有server都生效。
```nginx
http {
    server_tokens off;
}
```

```bash
$ curl -I www.etiantian.org
HTTP/1.1 200 OK
Server: nginx # 没有显示版本号
Date: Tue, 03 Apr 2018 12:22:54 GMT
Content-Type: text/html
Content-Length: 12
Last-Modified: Tue, 03 Apr 2018 12:03:34 GMT
Connection: keep-alive
ETag: "5ac36d96-c"
Accept-Ranges: bytes
```

- [server_tokens](http://nginx.org/en/docs/http/ngx_http_core_module.html#server_tokens)

### 8.1.2　更改源码隐藏Nginx软件名及版本号

跳过

### 8.1.3　更改Nginx服务的默认用户

一般情况下，Nginx服务启动后，默认使用的用户是nobody。

为Nginx服务建立新用户
```bash
useradd nginx -s /sbin/nologin -M
```

更改Nginx服务默认使用的用户，方法有两种：

第一种为直接更改配置文件参数
```nginx
# `nginx.conf`
user nginx nginx;
```

- [user](http://nginx.org/en/docs/ngx_core_module.html#user)

第二种为直接在编译Nginx软件时指定编译的用户和组
```bash
./configure --user=nginx --group=nginx
```

检查效果
```bash
$ ps -ef | grep nginx | grep -v grep
root      1334     1  0 11:59 ?        00:00:00 nginx: master process nginx
nginx     1440  1334  0 12:22 ?        00:00:00 nginx: worker process
```

## 8.2　根据参数优化Nginx服务性能

### 8.2.1　优化Nginx服务的worker进程个数

**测试环境：DigitalOcean上2个1核CPU的主机**

查看CPU总个数
```bash
$ grep 'physical id' /proc/cpuinfo | sort | uniq | wc -l
2
```

查看CPU总核数
```bash
$ grep processor /proc/cpuinfo | wc -l
2
```

配置与总核数相同的工作进程数
```nginx
worker_processes 2;
```

检查配置结果
```bash
$ ps -ef | grep nginx | grep -v grep
root     10494     1  0 13:24 ?        00:00:00 nginx: master process nginx
# 2个工作进程
nginx    10563 10494  0 13:26 ?        00:00:00 nginx: worker process
nginx    10564 10494  0 13:26 ?        00:00:00 nginx: worker process
```

- [worker_processes](http://nginx.org/en/docs/ngx_core_module.html#worker_processes)

### 8.2.2　优化绑定不同的Nginx进程到不同的CPU上

学习到这