# [Node.js 文件系统](http://www.runoob.com/nodejs/nodejs-fs.html)

## 异步和同步

`./input.txt`
```
菜鸟教程官网地址：www.runoob.com
文件读取实例
```

`./demo.js`
```javascript
var fs = require('fs');

fs.readFile('input.txt', function (err, data) {
    if (err) {
        return console.error(err);
    }
    console.log(data.toString());
});

var data = fs.readFileSync('input.txt');
console.log(data.toString());
```

## 打开文件

```javascript
var fs = require('fs');

fs.open('input.txt', 'r+', function (err, fd) {
    if (err) {
        return console.error(err);
    }
    console.log('文件打开成功');
});
```

输出结果：
```
$ node demo.js
文件打开成功
```

## 获取文件信息

```javascript
var fs = require('fs');

fs.stat('input.txt', function (err, stats) {
    console.log(stats.isFile());
})
```

输出结果：
```
$ node demo.js
true
```

## 获取文件信息

```javascript
var fs = require('fs');

fs.stat('input.txt', function (err, stats) {
    if (err) {
        return console.error(err);
    }
    console.log(stats);
    console.log(stats.isFile());
    console.log(stats.isDirectory());
})
```

输出结果：
```
$ node demo.js
Stats {
  dev: 637946826,
  mode: 33206,
  nlink: 1,
  uid: 0,
  gid: 0,
  rdev: 0,
  blksize: undefined,
  ino: 83316593106394620,
  size: 61,
  blocks: undefined,
  atimeMs: 1513579903634.8674,
  mtimeMs: 1513580046846.8674,
  ctimeMs: 1513580046846.8674,
  birthtimeMs: 1513579903558.8674,
  atime: 2017-12-18T06:51:43.635Z,
  mtime: 2017-12-18T06:54:06.847Z,
  ctime: 2017-12-18T06:54:06.847Z,
  birthtime: 2017-12-18T06:51:43.559Z }
true
false
```

## 写入文件

```javascript
var fs = require('fs');

fs.writeFile('input.txt', 'Hello, Node.js!', function (err) {
    if (err) {
        return console.error(err);
    }
    fs.readFile('input.txt', function (err, data) {
        if (err) {
            return console.log(err);
        }
        console.log(data.toString());
    });
});
```

输出结果：
```
$ node demo.js
Hello, Node.js!
```

## 读取文件

```javascript
var fs = require('fs');

var buf = new Buffer(1024);

fs.open('input.txt', 'r+', function (err, fd) {
    if (err) {
        return console.error(err);
    }

    fs.read(fd, buf, 0, buf.length, 0, function (err, bytes) {
        if (err) {
            console.log(err);
        }
        console.log(bytes + ' 字节被读取');
        if (bytes > 0) {
            console.log(buf.slice(0, bytes).toString());
        }
    });
});
```

输出结果：
```
$ node demo.js
41 字节被读取
菜鸟教程官网地址：www.runoob.com
```

## 关闭文件

```javascript
var fs = require('fs');

var buf = new Buffer(1024);

fs.open('input.txt', 'r+', function (err, fd) {
    if (err) {
        return console.error(err);
    }

    fs.read(fd, buf, 0, buf.length, 0, function (err, bytes) {
        if (err) {
            console.log(err);
        }
        console.log(bytes + ' 字节被读取');
        if (bytes > 0) {
            console.log(buf.slice(0, bytes).toString());
        }

        fs.close(fd, function (err) {
            if (err) {
                console.log(err);
            }
            console.log('文件关闭成功');
        });
    });
});
```

输出结果：
```
$ node demo.js
41 字节被读取
菜鸟教程官网地址：www.runoob.com
文件关闭成功
```

## 截取文件

`./input.txt`
```
www.runoob.com
```

`./demo.js`
```javascript
var fs = require('fs');

var buf = new Buffer(1024);

fs.open('input.txt', 'r+', function (err, fd) {
    if (err) {
        return console.error(err);
    }

    fs.ftruncate(fd, 10, function (err) {
        if (err) {
            console.log(err);
        }
        
        fs.read(fd, buf, 0, buf.length, 0, function (err, bytes) {
            if (err) {
                console.log(err);
            }
            console.log(bytes + ' 字节被读取');
            if (bytes > 0) {
                console.log(buf.slice(0, bytes).toString());
            }

            fs.close(fd, function (err) {
                if (err) {
                    console.log(err);
                }
                console.log('文件关闭成功');
            });
        });
    });
});
```

输出结果：
```
$ node demo.js
10 字节被读取
www.runoob
文件关闭成功
```

## 删除文件

```javascript
var fs = require('fs');

fs.unlink('input.txt', function (err) {
    if (err) {
        return console.error(err);
    }
    console.log('文件删除成功');
});
```

输出结果：
```
$ node demo.js
文件删除成功
```

## 创建目录

```javascript
var fs = require('fs');

fs.mkdir('/tmp/dir1', function (err) {
    if (err) {
        return console.error(err);
    }
    console.log('目录创建成功');
});
```

输出结果：
```
$ node demo.js
目录创建成功
```

## 读取目录

```javascript
var fs = require('fs');

fs.readdir('/tmp/', function (err, files) {
    if (err) {
        return console.error(err);
    }
    files.forEach(function (file) {
        console.log(file);
    });
});
```

输出结果：
```
$ node demo.js
input.txt
```

## 删除目录

```javascript
var fs = require('fs');

fs.rmdir('/tmp/dir1/', function (err) {
    if (err) {
        return console.error(err);
    }
    fs.readdir('/tmp/', function (err, files) {
        if (err) {
            return console.error(err);
        }
        files.forEach(function (file) {
            console.log(file);
        });
    })
});
```