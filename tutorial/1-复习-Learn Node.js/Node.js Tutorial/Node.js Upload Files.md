# [Node.js Upload Files](https://www.w3schools.com/nodejs/nodejs_uploadfiles.asp)

```bash
npm install formidable
```

```javascript
var http = require('http');
var formidable = require('formidable');
var fs = require('fs');

http.createServer(function (req, res) {
    if (req.url == '/fileupload') {
        var form = new formidable.IncomingForm();
        form.parse(req, function (err, fields, files) {
            var oldpath = files.filetoupload.path;
            var newpath = '/tmp/' + files.filetoupload.name;
            fs.rename(oldpath, newpath, function (err) {
                if (err) {
                    throw err;
                }
                res.write('File uploaded');
                res.end();
            });
        })
    } else {
        res.writeHead(200, {'Content-Type': 'text/html'});
        res.write('<form action="fileupload" method="post" enctype="multipart/form-data">');
        res.write('<input type="file" name="filetoupload"><br>');
        res.write('<input type="submit">');
        res.write('<form>');
        return res.end();
    }
}).listen(8080);
```

```bash
node server.js
```

**API**

[formidable](https://www.npmjs.com/package/formidable)

[fs.rename(oldPath, newPath, callback)](https://nodejs.org/dist/latest-v8.x/docs/api/fs.html#fs_fs_rename_oldpath_newpath_callback)