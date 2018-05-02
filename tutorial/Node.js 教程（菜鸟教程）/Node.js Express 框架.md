# [Node.js Express 框架](http://www.runoob.com/nodejs/nodejs-express-framework.html)

## 安装 Express

```bash
npm install express --save
npm install body-parser --save
npm install cookie-parser --save
npm install multer --save
```

## 第一个 Express 框架实例

```javascript
var express = require('express');

var app = express();
app.get('/', function (req, res) {
    res.send('Hello, Express!');
});

var server = app.listen(8080, function () {
    var host = server.address().address;
    var port = server.address().port;
    console.log("Server is running at http://%s:%s", host, port);
});
```

输出结果：
```
$ node server.js
Server is running at http://:::8080
```

打开网址：http://localhost:8080/

## 路由

```javascript
var express = require('express');
var app = express();

app.get('/', function (req, res) {
    console.log('主页 GET 请求');
    res.send('Hello GET');
});

app.post('/', function (req, res) {
    console.log('主页 POST 请求');
    res.send('Hello POST');
});

app.get('/del_user', function (req, res) {
    console.log('/del_user 响应 DELETE 请求');
    res.send('删除页面');
});

app.get('/list_user', function (req, res) {
    console.log('/list_user GET 请求');
    res.send('用户列表页面');
});

app.get('/ab*cd', function (req, res) {
    console.log('/ab*cd GET 请求');
    res.send('正则匹配');
});

var server = app.listen(8080, function () {
    var host = server.address().address;
    var port = server.address().port;
    console.log("Server is running at http://%s:%s", host, port);
});
```

启动服务：
```
$ node server.js
Server is running at http://:::8080
```

打开以下网址：

http://localhost:8080 \
http://localhost:8080/del_user \
http://localhost:8080/list_user \
http://localhost:8080/abcd \
http://localhost:8080/abcdefg

## 静态文件

`./server.js`
```javascript
var express = require('express');
var app = express();

app.use(express.static('public'));

app.get('/', function (req, res) {
    res.send('Hello, Express!');
});

var server = app.listen(8080, function () {
    var host = server.address().address;
    var port = server.address().port;
    console.log("Server is running at http://%s:%s", host, port);
});
```

`./public/`
```
$ tree public/
public/
└── images
    └── logo.png
```

启动服务：
```
$ node server.js
Server is running at http://:::8080
```

打开网址：\
http://localhost:8080/images/logo.png

## GET 方法

`./index.html`
```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
</head>
<body>
    <form action="process_get" method="get">
        First Name: <input type="text" name="firstname"><br>
        Last Name: <input type="text" name="lastname"><br>
        <input type="submit" value="Submit">
    </form>
</body>
</html>
```

`./server.js`
```javascript
var express = require('express');
var app = express();

app.use(express.static('public'));

app.get('/index.html', function (req, res) {
    res.sendFile(__dirname + "/index.html")
});

app.get('/process_get', function (req, res) {
    var response = {
        "firstname": req.query.firstname,
        "lastname": req.query.lastname
    };
    console.log(response);
    res.end(JSON.stringify(response));
});

var server = app.listen(8080, function () {
    var host = server.address().address;
    var port = server.address().port;
    console.log("Server is running at http://%s:%s", host, port);
});
```

启动服务：
```
$ node server.js
Server is running at http://:::8080
```

打开网址：
http://localhost:8080/index.html

## POST 方法

`./index.html`
```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
</head>
<body>
    <form action="process_post" method="post">
        First Name: <input type="text" name="firstname"><br>
        Last Name: <input type="text" name="lastname"><br>
        <input type="submit" value="Submit">
    </form>
</body>
</html>
```

`./server.js`
```javascript
var express = require('express');
var app = express();
var bodyParser = require('body-parser');

var urlencodedParser = bodyParser.urlencoded({extended: false});

app.use(express.static('public'));

app.get('/index.html', function (req, res) {
    res.sendFile(__dirname + "/index.html")
});

app.post('/process_post', urlencodedParser, function (req, res) {
    var response = {
        "firstname": req.body.firstname,
        "lastname": req.body.lastname
    };
    console.log(response);
    res.end(JSON.stringify(response));
});

var server = app.listen(8080, function () {
    var host = server.address().address;
    var port = server.address().port;
    console.log("Server is running at http://%s:%s", host, port);
});
```

启动服务：
```
$ node server.js
Server is running at http://:::8080
```

打开网址：

http://localhost:8080/index.html

## 文件上传

`./index.html`
```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
</head>
<body>
    <h3>文件上传：</h3>
    选择一个文件上传：<br>
    <form action="/file_upload" method="post" enctype="multipart/form-data">
        <input type="file" name="image" size="50">
        <br>
        <input type="submit" value="上传文件">
    </form>
</body>
</html>
```

`./server.js`
```javascript
var express = require('express');
var bodyParser = require('body-parser');
var multer = require('multer');
var fs = require('fs');

var app = express();

app.use(express.static('public'));
app.use(bodyParser.urlencoded({extended: false}));
app.use(multer({dest: '/tmp/'}).array('image'));

app.get('/index.html', function (req, res) {
    res.sendFile(__dirname + "/index.html")
});

app.post('/file_upload', function (req, res) {
    console.log(req.files[0]);
    var dest_file = __dirname + '/' + req.files[0].originalname;
    fs.readFile(req.files[0].path, function (err, data) {
        if (err) {
            console.log(err);
        } else {
            response = {
                message: 'File uploaded successfully',
                filename: req.files[0].originalname
            }
        }
        console.log(response);
        res.end(JSON.stringify(response));
    });
});

var server = app.listen(8080, function () {
    var host = server.address().address;
    var port = server.address().port;
    console.log("Server is running at http://%s:%s", host, port);
});
```

启动服务：
```
$ node server.js
Server is running at http://:::8080
{ fieldname: 'image',
  originalname: 'sample.txt',
  encoding: '7bit',
  mimetype: 'text/plain',
  destination: '/tmp/',
  filename: '2261b39800bc52cf41ff98bec4b2f4e1',
  path: '\\tmp\\2261b39800bc52cf41ff98bec4b2f4e1',
  size: 0 }
{ message: 'File uploaded successfully',
  filename: 'sample.txt' }
```

打开网址：
http://localhost:8080/index.html

## Cookie 管理

```javascript
var express = require('express');
var cookieParser = require('cookie-parser');

var app = express();
app.use(cookieParser());

app.get('/', function (req, res) {
    console.log('Cookies: ', req.cookies);
});

app.listen(8080);
```

启动服务：
```
$ node server.js
Cookies:  {}
```

打开网址：
http://localhost:8080