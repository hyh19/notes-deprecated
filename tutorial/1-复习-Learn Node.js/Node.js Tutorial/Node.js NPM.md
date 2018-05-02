# [Node.js NPM](https://www.w3schools.com/nodejs/nodejs_npm.asp)

```bash
npm install upper-case
```

`./server.js`
```javascript
var http = require('http');
var uc = require('upper-case');

http.createServer(function (req, res) {
    res.writeHead(200, {'Content-Type': 'text/html'});
    res.write(uc('Hello World!'));
    res.end();
}).listen(8080);
```

```bash
node server.js
```

http://localhost:8080

**API**

[upper-case](https://www.npmjs.com/package/upper-case)

