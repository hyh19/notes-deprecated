# [Node.js 创建第一个应用](http://www.runoob.com/nodejs/nodejs-http-server.html)

```javascript
var http = require('http');

http.createServer(function (request, response) {
    response.writeHead(200, {'Content-Type': 'text/plain'});
    response.end('Hello World\n');
}).listen(8888);

console.log('Server running at http://127.0.0.1:8888/');
```
