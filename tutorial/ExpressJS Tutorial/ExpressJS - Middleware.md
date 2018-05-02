# [ExpressJS - Middleware](https://www.tutorialspoint.com/expressjs/expressjs_middleware.htm)

```javascript
var express = require('express')
var app = express()

app.use(function (req, res, next) {
    console.log("A new request received at " + Date.now())
    next()
})

app.listen(3000)
```

Client
```
$ curl "http://localhost:3000"
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<title>Error</title>
</head>
<body>
<pre>Cannot GET /</pre>
</body>
</html>
```

Server
```
$ nodemon index.js
A new request received at 1513758468712
A new request received at 1513758499879
A new request received at 1513758512000
```

**API**

http://expressjs.com/en/4x/api.html#app.use

**Guide**

http://expressjs.com/en/guide/using-middleware.html

```javascript
var express = require('express')
var app = express()

app.use('/things', function (req, res, next) {
    console.log("A new request received at " + Date.now())
    next()
})

app.get('/things', function (req, res) {
    res.send('Things')
})

app.listen(3000)
```

Client
```
$ curl "http://localhost:3000/things"
Things
$ curl "http://localhost:3000/things"
Things
```

Server
```
$ nodemon index.js
A new request received at 1513758772665
A new request received at 1513758791176
```

## Order of Middleware Calls

```javascript
var express = require('express')
var app = express()

app.use(function (req, res, next) {
    console.log("Start: " + Date.now())
    next()
})

app.get('/', function (req, res, next) {
    res.send("Middle: " + Date.now())
    next()
})

app.use('/', function (req, res) {
    console.log("End: " + Date.now())
})

app.listen(3000)
```

```
$ curl "http://localhost:3000"
Middle: 1513759332296
```

```
$ nodemon index.js
Start: 1513759332295
End: 1513759332319
```

## Third Party Middleware

http://expressjs.com/en/resources/middleware.html