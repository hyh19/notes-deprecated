# [NPM 使用介绍](http://www.runoob.com/nodejs/nodejs-npm.html)

测试是否成功安装
```bash
npm -v
```

升级
```bash
npm install npm -g
```

全局安装与本地安装
```bash
npm install express
npm install express -g
```

导入模块
```javascript
var express = require('express');
```

查看所有全局安装的模块
```bash
npm list -g
```

查看某个模块的版本号
```bash
npm list express
```

卸载模块
```bash
npm uninstall express
```

查看已安装的模块
```bash
npm ls
```

更新模块
```bash
npm update express
```

搜索模块
```bash
npm search express
```

创建模块
```bash
npm init
```

在 `npm` 资源库中注册用户
```bash
npm adduser
```

发布模块
```bash
npm publish
```

安装淘宝 `npm` 镜像工具
```bash
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

使用淘宝 `npm` 镜像工具安装模块
```bash
cnpm install express
```