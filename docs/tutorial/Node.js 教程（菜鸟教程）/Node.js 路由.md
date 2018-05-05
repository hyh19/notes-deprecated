# [Node.js 路由](http://www.runoob.com/nodejs/nodejs-router.html)

`./server.js`
```javascript
var http = require('http');
var url = require('url');

function start(route) {

    function onRequest(request, response) {

        var pathname = url.parse(request.url).pathname;

        console.log('Request for ' + pathname + ' received.');

        route(pathname);

        response.writeHead(200, {'Content-Type': 'text/plain'});

        response.write('Hello World');

        response.end();
    }

    http.createServer(onRequest).listen(8888);

    console.log('Server has started.');
}

exports.start = start;
```

`./router.js`
```javascript
function route(pathname) {
    console.log('About to route a request for ' + pathname);
}

exports.route = route;
```

`./index.js`
```javascript
var server = require('./server');
var router = require('./router');

server.start(router.route);
```