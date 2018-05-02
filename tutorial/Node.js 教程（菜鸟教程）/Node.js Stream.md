# [Node.js Stream](http://www.runoob.com/nodejs/nodejs-stream.html)

## 从流中读取数据

`./input.txt`
```
菜鸟教程官网地址：www.runoob.com
```

`./main.js`
```javascript
var fs = require('fs');

var data = '';

var readStream = fs.createReadStream('input.txt');

readStream.setEncoding('utf8');

readStream.on('data', function (chunk) {
    data += chunk;
});

readStream.on('end', function () {
    console.log(data);
});

readStream.on('error', function (err) {
    console.log(err.stack);
});
```

输出结果：
```
$ node main.js 
菜鸟教程官网地址：www.runoob.com

$ node main.js 
Error: ENOENT: no such file or directory, open 'input.txt'
```

## 写入流

```javascript
var fs = require('fs');

var data = '菜鸟教程官网地址：www.runoob.com';

var writerStream = fs.createWriteStream('output.txt');

writerStream.write(data, 'utf8');

writerStream.end();

writerStream.on('finish', function () {
    console.log('finish');
});

writerStream.on('error', function (err) {
    console.log(err.stack);
});
```

输出结果：
```
$ node main.js 
finish

$ cat output.txt 
菜鸟教程官网地址：www.runoob.comY
```

### 管道流

```javascript
var fs = require('fs');

var readerStream = fs.createReadStream('input.txt')

var writerStream = fs.createWriteStream('output.txt');

readerStream.pipe(writerStream);
```

输出结果：
```
$ node main.js 
$ cat output.txt 
菜鸟教程官网地址：www.runoob.com
```

## 链式流

`./compress.js`
```javascript
var fs = require('fs');
var zlib = require('zlib');

fs.createReadStream('input.txt')
.pipe(zlib.createGzip()).
pipe(fs.createWriteStream('input.txt.gz'));
```

`./decompress.js`
```javascript
var fs = require('fs');
var zlib = require('zlib');

fs.createReadStream('input.txt.gz')
.pipe(zlib.createGunzip())
.pipe(fs.createWriteStream('input.txt'));
```