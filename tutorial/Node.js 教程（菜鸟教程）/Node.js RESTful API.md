# [Node.js RESTful API](http://www.runoob.com/nodejs/nodejs-restful-api.html)

## 创建 RESTful

`./users.json`
```json
{
   "user1" : {
      "name" : "mahesh",
      "password" : "password1",
      "profession" : "teacher",
      "id": 1
   },
   "user2" : {
      "name" : "suresh",
      "password" : "password2",
      "profession" : "librarian",
      "id": 2
   },
   "user3" : {
      "name" : "ramesh",
      "password" : "password3",
      "profession" : "clerk",
      "id": 3
   }
}
```

### 获取用户列表

```javascript
var express = require('express');
var fs = require('fs');

var app = express();

app.get('/listUsers', function (req, res) {
    fs.readFile(__dirname + '/users.json', 'utf8', function (err, data) {
        console.log(data);
        res.end(data);
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
```

打开网址：http://localhost:8080/listUsers

### 添加用户

```javascript
var express = require('express');
var fs = require('fs');

var app = express();

var user = {
    'user4': {
        'name': 'mohit',
        'password': 'password4',
        'profession': 'teacher',
        'id': 4
    }
};

app.get('/addUser', function (req, res) {
    fs.readFile(__dirname + '/users.json', 'utf8', function (err, data) {
        data = JSON.parse(data);
        data['user4'] = user['user4'];
        console.log(data);
        res.end(JSON.stringify(data));
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
```

打开网址：http://localhost:8080/addUser

### 显示用户详情

```javascript
var express = require('express');
var fs = require('fs');

var app = express();

app.get('/:id', function (req, res) {
    fs.readFile(__dirname + '/users.json', 'utf8', function (err, data) {
        data = JSON.parse(data);
        var user = data['user' + req.params.id];
        console.log(user);
        res.end(JSON.stringify(user));
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
```

打开网址：http://localhost:8080/2

### 删除用户

```javascript
var express = require('express');
var fs = require('fs');

var app = express();

var id = 2;

app.get('/deleteUser', function (req, res) {
    fs.readFile(__dirname + '/users.json', 'utf8', function (err, data) {
        data = JSON.parse(data);
        delete data['user' + 2];
        console.log(data);
        res.end(JSON.stringify(data));
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
```

打开网址：http://localhost:8080/deleteUser