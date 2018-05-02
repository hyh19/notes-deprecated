# [Node.js File System Module](https://www.w3schools.com/nodejs/nodejs_filesystem.asp)

`./index.html`
```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
</head>
<body>
    <h1>My Header</h1>
    <p>My paragraph.</p>
</body>
</html>
```

`./server.js`
```javascript
var http = require('http');
var fs = require('fs');

http.createServer(function (req, res) {
    fs.readFile('index.html', function (err, data) {
        res.writeHead(200, {'Content-Type': 'text/html'});
        res.write(data);
        res.end()
    });
}).listen(8080);
```

```bash
node server.js
```

http://localhost:8080

**API**

[File System](https://nodejs.org/api/fs.html#fs_file_system)

[fs.readFile(path[, options], callback)](https://nodejs.org/api/fs.html#fs_fs_readfile_path_options_callback)
