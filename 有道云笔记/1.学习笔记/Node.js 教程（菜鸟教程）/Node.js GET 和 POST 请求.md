# [Node.js GET/POST 请求](http://www.runoob.com/nodejs/node-js-get-post.html)

## 获取 GET 请求内容

```javascript
var http = require('http');
var url = require('url');
var util = require('util');

http.createServer(function (req, res) {
    res.writeHead(200, {'Content-Type': 'text/plain; charset=utf-8'});
    res.end(util.inspect(url.parse(req.url, true)));
}).listen(3000);
```

启动服务器：
```
$ node server.js
```

打开网址：http://localhost:3000/user?name=%E8%8F%9C%E9%B8%9F%E6%95%99%E7%A8%8B&url=www.runoob.com

输出结果：
```
Url {
  protocol: null,
  slashes: null,
  auth: null,
  host: null,
  port: null,
  hostname: null,
  hash: null,
  search: '?name=%E8%8F%9C%E9%B8%9F%E6%95%99%E7%A8%8B&url=www.runoob.com',
  query: { name: '菜鸟教程', url: 'www.runoob.com' },
  pathname: '/user',
  path: '/user?name=%E8%8F%9C%E9%B8%9F%E6%95%99%E7%A8%8B&url=www.runoob.com',
  href: '/user?name=%E8%8F%9C%E9%B8%9F%E6%95%99%E7%A8%8B&url=www.runoob.com' }
```

### 获取 URL 的参数

```javascript
var http = require('http');
var url = require('url');
var util = require('util');

http.createServer(function (req, res) {
    res.writeHead(200, {'Content-Type': 'text/plain; charset=utf-8'});
    var params = url.parse(req.url, true).query;
    res.write('name: ' + params.name);
    res.write("\n");
    res.write('url: ' + params.url);
    res.end();
}).listen(3000);
```

启动服务器：
```
$ node server.js
```

打开网址：http://localhost:3000/user?name=%E8%8F%9C%E9%B8%9F%E6%95%99%E7%A8%8B&url=www.runoob.com

输出结果：
```
name: 菜鸟教程
url: www.runoob.com
```

## 获取 POST 请求内容

```javascript
var http = require('http');
var querystring = require('querystring');

var postHTML = 
    '<html><head><meta charset="utf-8"><title>菜鸟教程 Node.js 实例</title></head>' +
    '<body>' +
    '<form method="post">' +
    '网站名： <input name="name"><br>' +
    '网站 URL： <input name="url"><br>' +
    '<input type="submit">' +
    '</form>' +
    '</body></html>';

http.createServer(function (req, res) {
    var body = '';
    req.on('data', function (chunk) {
        body += chunk;
    });
    req.on('end', function () {
        body = querystring.parse(body);
        res.writeHead(200, {'Content-Type': 'text/html; charset=utf8'});
        if (body.name && body.url) {
            res.write('网站名：' + body.name);
            res.write('<br>');
            res.write('网站 URL：' + body.url);
        } else {
            res.write(postHTML);
        }
        res.end();
    });
}).listen(3000);
```

启动服务
```
$ node server.js
```

打开网址：http://localhost:3000/