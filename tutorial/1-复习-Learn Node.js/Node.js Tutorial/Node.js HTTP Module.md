# [Node.js HTTP Module](https://www.w3schools.com/nodejs/nodejs_http.asp)

`./server.js`
```javascript
var http = require('http');

http.createServer(function (req, res) {
    res.write('Hello World!');
    res.end();
}).listen(8080);
```

```bash
node server.js
```

http://localhost:8080

**API**

[response.write(chunk[, encoding][, callback])](https://nodejs.org/api/http.html#http_response_write_chunk_encoding_callback)