[toc]

# RVM Manual

http://rvm.io/

https://github.com/rvm/rvm

The Basics of RVM
http://rvm.io/rvm/basics

Installing RVM
http://rvm.io/rvm/install

Upgrading RVM
http://rvm.io/rvm/upgrading

Examples of using RVM
http://rvm.io/workflow/examples

Cheat Sheet
http://cheat.errtheblog.com/s/rvm

RVM CLI Usage
http://rvm.io/rvm/cli

RVM Best Practices
http://rvm.io/rvm/best-practices

Myths
http://rvm.io/rvm/myths

## Installation


```
$ gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
$ \curl -sSL https://get.rvm.io | bash -s stable
```


更新RVM
$ rvm get stable

安装Ruby的依赖环境
$ rvm requirements

列出可安装的Ruby版本
$ rvm list known

Installing Rubies
http://rvm.io/rubies/installing

Upgrading Rubies
http://rvm.io/rubies/upgrading

安装Ruby
$ rvm install 1.9.3

Listing RVM installed rubies
http://rvm.io/rubies/list

列出已安装的Ruby
$ rvm list

Setting the default Ruby
http://rvm.io/rubies/default

设置Ruby版本
# 切换版本
$ rvm use 2.4

# 切换版本并设置为默认
$ rvm --default use 2.1.1

# 使用系统预装版本
$ rvm use system

Removing Rubies
http://rvm.io/rubies/removing

Uninstalling Rubies
http://rvm.io/rubies/uninstalling

卸载Ruby
$ rvm uninstall 2.0.0
