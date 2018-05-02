# [Node.js Get Started](https://www.w3schools.com/nodejs/nodejs_get_started.asp)

```javascript
var http = require('http');

http.createServer(function (req, res) {
    res.writeHead(200, {'Content-Type': 'text/html'});
    res.end('Hello World!');
}).listen(8080);
```

```bash
node server.js
```

[module.require(id)](https://nodejs.org/api/modules.html#modules_module_require_id)

[HTTP](https://nodejs.org/api/http.html#http_http)

[http.createServer([requestListener])](https://nodejs.org/api/http.html#http_http_createserver_requestlistener)

[Class: http.Server](https://nodejs.org/api/http.html#http_class_http_server)

[server.listen([port][, host][, backlog][, callback])](https://nodejs.org/api/net.html#net_server_listen_port_host_backlog_callback)

[Class: http.ServerResponse](https://nodejs.org/api/http.html#http_class_http_serverresponse)

[response.writeHead(statusCode[, statusMessage][, headers])](https://nodejs.org/api/http.html#http_response_writehead_statuscode_statusmessage_headers)

[response.end([data][, encoding][, callback])](https://nodejs.org/api/http.html#http_response_end_data_encoding_callback)

[Event: 'request'](https://nodejs.org/api/http.html#http_event_request)
