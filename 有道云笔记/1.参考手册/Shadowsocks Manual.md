[TOC]

# Shadowsocks Manual

https://shadowsocks.org/

https://github.com/shadowsocks/shadowsocks

GFWList
https://github.com/gfwlist/gfwlist

User Rules
https://adblockplus.org/en/filter-cheatsheet

## Servers

### Install
https://github.com/shadowsocks/shadowsocks/tree/master#install

#### Ubuntu

[install-shadowsocks-apt.sh](https://github.com/mrhuangyuhui/shadowsocks/blob/master/install-shadowsocks-apt.sh)
```bash
curl -L https://github.com/mrhuangyuhui/shadowsocks/raw/master/install-shadowsocks-apt.sh | bash
```

```bash
apt-get install python-pip
pip install git+https://github.com/shadowsocks/shadowsocks.git@master
```

**CentOS**
```bash
yum install python-setuptools && easy_install pip
pip install git+https://github.com/shadowsocks/shadowsocks.git@master
```

**Auto Install Shadowsocks Server for CentOS/Debian/Ubuntu**

https://github.com/teddysun/shadowsocks_install

### [Usage](https://github.com/shadowsocks/shadowsocks/tree/master#usage)

```
$ ssserver -p 443 -k password -m aes-256-cfb
```

To run in the background:
```
$ sudo ssserver -p 443 -k password -m aes-256-cfb --user nobody -d start
```

To stop:
```
$ sudo ssserver -d stop
```

## [Clients](https://shadowsocks.org/en/download/clients.html)

### Mac OS X

ShadowsocksX-NG
https://github.com/shadowsocks/ShadowsocksX-NG/releases

### Windows

shadowsocks-win
https://github.com/shadowsocks/shadowsocks-windows/releases

### Android

shadowsocks-android
https://github.com/shadowsocks/shadowsocks-android/releases

## Documentation

https://github.com/shadowsocks/shadowsocks/wiki