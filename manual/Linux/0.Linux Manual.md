[TOC]

# Linux Manual

https://www.kernel.org/

http://www.gnu.org/

https://github.com/judasn/Linux-Tutorial

https://github.com/arismelachroinos/lscript

lsb_release 查看系统版本
```
# 安装
yum install -y redhat-lsb-core
```

tar
```bash
xz -d example.tar.xz && tar -xvf example.tar
```

## Packages

https://pkgs.org/

## [Coreutils - GNU core utilities](https://www.gnu.org/software/coreutils/manual/coreutils.html)

### [21 System context](https://www.gnu.org/software/coreutils/manual/coreutils.html#System-context)

#### [21.2 `arch`: Print machine hardware name](https://www.gnu.org/software/coreutils/manual/coreutils.html#arch-invocation)

`arch` prints the machine hardware name, and is equivalent to `uname -m`.
```bash
$ arch
x86_64
```

#### [21.3 nproc: Print the number of available processors](https://www.gnu.org/software/coreutils/manual/coreutils.html#nproc-invocation)

Print the number of processing units available to the current process, which may be less than the number of online processors.
```bash
$ nproc
1
```

#### [21.4 `uname`: Print system information](https://www.gnu.org/software/coreutils/manual/coreutils.html#uname-invocation)

Print all information
```
$ uname -a
Linux debian-512mb-sgp1-01 3.16.0-4-amd64 #1 SMP Debian 3.16.43-2+deb8u2 (2017-06-26) x86_64 GNU/Linux
```

Print the kernel release
```bash
$ uname -r
3.16.0-4-amd64
```

Print the kernel version
```
$ uname -v
#1 SMP Debian 3.16.43-2+deb8u2 (2017-06-26)
```

#### [21.5 `hostname`: Print or set system name](https://www.gnu.org/software/coreutils/manual/coreutils.html#hostname-invocation)

Get the hostname 
```bash
$ hostname
mysql.example.com
$ cat /etc/hostname
mysql.example.com
```

Set the hostname. Note, that  this  is  effective  only  until  the  next reboot. Edit /etc/hostname for permanent change.
```bash
$ hostname redis.example.com
$ hostname
redis.example.com
$ cat /etc/hostname
mysql.example.com

$ echo redis.example.com > /etc/hostname
$ reboot ## REBOOT!!! ##
$ hostname
redis.example.com
```

#### [21.7 `uptime`: Print system uptime and load](https://www.gnu.org/software/coreutils/manual/coreutils.html#uptime-invocation)

`uptime` prints the current time, the system's uptime, the number of logged-in users and the current load average.
```bash
$ uptime
 03:48:42 up 28 min,  2 users,  load average: 0.00, 0.00, 0.00
```

```bash
# 读取系统平均负载。后面的分数，分母表示系统进程总数，分子表示正在运行的进程数，最后一个数字表示最近运行的进程 ID。
$ cat /proc/loadavg
0.00 0.00 0.00 1/74 1248

# 读取 CPU 核心数目
$ grep 'model name' /proc/cpuinfo | wc -l
1
```

## Tutorials

http://linux.vbird.org/