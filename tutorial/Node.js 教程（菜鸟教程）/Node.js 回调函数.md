# [Node.js 回调函数](http://www.runoob.com/nodejs/nodejs-callback.html)

`./input.txt`
```
菜鸟教程官网地址：www.runoob.com
```

## 阻塞代码实例

`./main.js`
```javascript
var fs = require('fs');

var data = fs.readFileSync('input.txt');

console.log(data.toString());
console.log('程序执行结束');
```

## 非阻塞代码实例

`./main.js`
```javascript
var fs = require('fs');

fs.readFile('input.txt', function (err, data) {
    if (err) {
        return console.error(err);
    }
    console.log(data.toString());
});

console.log('程序执行结束');
```

**API**

[Console](https://nodejs.org/api/console.html)