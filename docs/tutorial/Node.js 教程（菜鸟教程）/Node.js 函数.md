# [Node.js 函数](http://www.runoob.com/nodejs/nodejs-function.html)

```javascript
function say(word) {
    console.log(word);
}

function execute(someFunction, value) {
    someFunction(value);
}

execute(say, 'Hello');
```

输出结果：
```
$ node main.js 
Hello
```

## 匿名函数

```javascript
function execute(someFunction, value) {
    someFunction(value);
}

execute(function (word) { console.log(word) }, 'Hello');
```

输出结果：
```
$ node main.js 
Hello
```

## 函数传递是如何让 HTTP 服务器工作的

```javascript
var http = require('http');

http.createServer(function (request, response) {
    response.writeHead(200, {'Content-Type': 'text/plain'});
    response.write('Hello World');
    response.end();
}).listen(8888);
```
或
```javascript
var http = require('http');

function onRequest(request, response) {
    response.writeHead(200, {'Content-Type': 'text/plain'});
    response.write('Hello World');
    response.end();
}

http.createServer(onRequest).listen(8888);
```

打开网址：http://localhost:8888