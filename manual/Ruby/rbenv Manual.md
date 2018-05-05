[toc]

# rbenv Manual

https://github.com/rbenv/rbenv

## Installation [>>](https://github.com/rbenv/rbenv#installation)


```bash
yum group install -y 'Development Tools'
```


Mac
```bash
brew install rbenv

# 初始化配置
rbenv init

# 添加 eval "$(rbenv init -)" 到文件 .bash_profile
echo 'eval "$(rbenv init -)"' >> ~/.bash_profile

# 另开一个终端，配置生效。

# 验证安装和配置结果
curl -fsSL https://github.com/rbenv/rbenv-installer/raw/master/bin/rbenv-doctor | bash
```

## Commands

```bash
# list all available versions:
$ rbenv install -l

# install a Ruby version:
$ rbenv install 2.0.0-p247
```
