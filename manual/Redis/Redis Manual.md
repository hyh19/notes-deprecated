[toc]

# Redis Manual

https://redis.io/

https://redis.io/documentation

https://github.com/antirez/redis

https://github.com/antirez/redis-doc

https://github.com/CodisLabs/codis

https://github.com/sohutv/cachecloud

https://github.com/vipshop/redis-migrate-tool

http://try.redis.io


## Installation

### Source [>>](https://redis.io/download)

需要完善的地方：
- 支持 Ubuntu 安装
- 安装 4.0.9 出现错误

https://github.com/antirez/redis#installing-redis

[install-redis-source.sh](https://github.com/mrhuangyuhui/redis/blob/master/install-redis-source.sh) 【快速安装脚本，安装完成后自动启动】
```bash
## CentOS 7.4 ##
curl -L https://github.com/mrhuangyuhui/redis/raw/master/install-redis-source.sh | bash
```

检查安装和启动情况
```bash
$ netstat -tulnp | grep redis
tcp        0      0 127.0.0.1:6379          0.0.0.0:*               LISTEN      15730/redis-server
```


启动服务端，端口默认是 6379。
```
$ redis-server
```

启动客户端，默认连接 6379 端口的服务端。
```bash
$ redis-cli
redis> set foo bar
OK
redis> get foo
"bar"
```

**配置 Redis 服务器，安装完成后使用，进行一些基本的配置。（快速安装脚本里已经进行了配置）[>>](https://github.com/antirez/redis/blob/unstable/utils/install_server.sh)**
```
################################################################################
#
# Service installer for redis server, runs interactively by default.
#
# To run this script non-interactively (for automation/provisioning purposes),
# feed the variables into the script. Any missing variables will be prompted!
# Tip: Environment variables also support command substitution (see REDIS_EXECUTABLE)
#
# Example:
#
# sudo REDIS_PORT=1234 \
# 		 REDIS_CONFIG_FILE=/etc/redis/1234.conf \
# 		 REDIS_LOG_FILE=/var/log/redis_1234.log \
# 		 REDIS_DATA_DIR=/var/lib/redis/1234 \
# 		 REDIS_EXECUTABLE=`command -v redis-server` ./utils/install_server.sh
#
# This generates a redis config file and an /etc/init.d script, and installs them.
#
# /!\ This script should be run as root
#
################################################################################
```

配置完成后的情况
```
Selected config:
Port           : 6379 # 监听端口
Config file    : /usr/local/redis/current/etc/6379.conf # 配置文件
Log file       : /usr/local/redis/current/log/6379.log # 日志文件
Data dir       : /usr/local/redis/current/data/6379 # 数据文件夹
Executable     : /usr/local/redis/current/bin/redis-server # 服务端程序
Cli Executable : /usr/local/redis/current/bin/redis-cli # 客户端程序
Copied /tmp/6379.conf => /etc/init.d/redis_6379 # 服务端启动脚本，可设置是否开机自动启动。
```

### YUM

查看安装包信息
```bash
yum info redis
```

```bash
## CentOS 6.9/7.4 ##
yum install -y epel-release
yum install -y redis
```

查看安装文件
```bash
$ rpm -ql redis
/etc/logrotate.d/redis
/etc/redis-sentinel.conf
/etc/redis.conf
...
```

卸载
```bash
$ yum erase redis
$ rpm -ql redis
package redis is not installed
```


### APT

```
## Ubuntu 14.04/16.04/17.04 ##
apt-get update
apt-get install -y redis-server
```

```
dpkg -L redis-server
```

## Clients

https://redis.io/clients

### Java

https://redis.io/clients#java

#### jedis

https://github.com/xetorthio/jedis

http://xetorthio.github.io/jedis/

https://github.com/xetorthio/jedis/wiki/Getting-started

```xml
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>2.9.0</version>
    <type>jar</type>
    <scope>compile</scope>
</dependency>
```
