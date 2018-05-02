# Mac 配置 PATH

```bash
# 注意：要用单引号，而不是双引号，因为双引号会解析全局变量 PATH 的值。
echo 'export PATH=${PATH}:~/zookeeper/current/bin' >> .bash_profile
```
