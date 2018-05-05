# [Node.js URL Module](https://www.w3schools.com/nodejs/nodejs_url.asp)

## The Built-in URL Module

```javascript
var url = require('url');
var adr = 'http://localhost:8080/default.htm?year=2017&month=february';

var q = url.parse(adr, true);

console.log(q.host);
console.log(q.pathname);
console.log(q.search);

var qdata = q.query;
console.log(qdata);
console.log(qdata.year);
console.log(qdata.month);
```

**API**

[URL](https://nodejs.org/dist/latest-v8.x/docs/api/url.html)

[The WHATWG URL API](https://nodejs.org/dist/latest-v8.x/docs/api/url.html#url_the_whatwg_url_api)

[Legacy URL API](https://nodejs.org/dist/latest-v8.x/docs/api/url.html#url_legacy_url_api)

[Legacy urlObject](https://nodejs.org/dist/latest-v8.x/docs/api/url.html#url_legacy_urlobject)

## Node.js File Server

`./summer.html`
```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
</head>
<body>
    <h1>Summer</h1>
    <p>I love the sun!</p>
</body>
</html>
```

`./winter.html`
```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
</head>
<body>
    <h1>Winter</h1>
    <p>I love the snow!</p>
</body>
</html>
```

`./server.js`
```javascript
var http = require('http');
var url = require('url');
var fs = require('fs');

http.createServer(function (req, res) {
    var q = url.parse(req.url, true);
    var filename = '.' + q.pathname;
    fs.readFile(filename, function (err, data) {
        if (err) {
            res.writeHead(404, {'Content-Type': 'text/html'});
            return res.end('404 Not Found');
        }
        res.writeHead(200, {'Content-Type': 'text/html'});
        res.write(data);
        return res.end();
    });
}).listen(8080);
```

```bash
node server.js
```

http://localhost:8080

http://localhost:8080/summer.html

http://localhost:8080/winter.html

**API**

[message.url](https://nodejs.org/dist/latest-v8.x/docs/api/http.html#http_message_url)