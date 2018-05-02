# [Node.js Web 模块](http://www.runoob.com/nodejs/nodejs-web-module.html)

## 使用 Node 创建 Web 服务器

`./server.js`
```javascript
var http = require('http');
var fs = require('fs');
var url = require('url');

http.createServer(function (request, response) {
    var pathname = url.parse(request.url).pathname;
    console.log('Request for ' + pathname + ' received.');

    fs.readFile(pathname.substr(1), function (err, data) {
        if (err) {
            console.log(err);
            response.writeHead(404, {'Content-Type': 'text/html; charset=utf8'});
        } else {
            response.writeHead(200, {'Content-Type': 'text/html; charset=utf8'});
            response.write(data.toString());
        }
        response.end();
    });

}).listen(8080);

console.log('Server running at http://127.0.0.1:8080/');
```

`./index.html`
```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
</head>
<body>
    <h1>我的第一个标题</h1>
    <p>我的第一个段落</p>
</body>
</html>
```

打开网址：http://localhost:8080/index.html

输出结果：
```
$ node server.js
Server running at http://127.0.0.1:8080/
Request for /index.html received.
```

## 使用 Node 创建 Web 客户端

`./client.js`
```javascript
var http = require('http');

var options = {
    host:'localhost',
    port: '8080',
    path: '/index.html'
};

var callback = function (response) {

    var body = '';

    response.on('data', function (data) {
        body += data;
    });

    response.on('end', function () {
        console.log(body);
    });
};

var req = http.request(options, callback);
req.end();
```

输出结果：
```
$ node client.js
<!DOCTYPE html>
<html>
<head>
    <title></title>
</head>
<body>
    <h1>我的第一个标题</h1>
    <p>我的第一个段落</p>
</body>
</html>
```