# [ExpressJS - Cookies](https://www.tutorialspoint.com/expressjs/expressjs_cookies.htm)

```
$ npm install --save cookie-parser
```

```javascript
var express = require('express')
var app = express()

var cookieParser = require('cookie-parser')
app.use(cookieParser())

app.get('/', function (req, res) {
    res.cookie('name', 'express').send('cookie set')
    console.log('Cookie: ', req.cookies)
})

app.listen(3000)
```

Client
```
$ curl "http://localhost:3000"
cookie set
```

Console
```
document.cookie
"name=express"
```

Server
```
[nodemon] starting `node index.js`
Cookie:  {}
Cookie:  { name: 'express' }
```

## Adding Cookies with Expiration Time

```javascript
var express = require('express')
var app = express()

var cookieParser = require('cookie-parser')
app.use(cookieParser())

app.get('/', function (req, res) {
    // res.cookie('hello', 'express', { expire: 360000 + Date.now() }).send('Adding Cookies with Expiration Time')
    res.cookie('hello', 'express', { maxAge: 360000 }).send('Adding Cookies with Expiration Time')
})

app.listen(3000)
```

Client
```
$ curl "http://localhost:3000"
Adding Cookies with Expiration Time
```

## Deleting Existing Cookies

```javascript
var express = require('express')
var app = express()

var cookieParser = require('cookie-parser')
app.use(cookieParser())

app.get('/', function (req, res) {
    res.cookie('foo', 'bar').send('cookie set')
})

app.get('/clear_cookie_foo', function (req, res) {
    res.clearCookie('foo')
    res.send('cookie foo cleared')
})

app.listen(3000)
```

Client
```
$ curl "http://localhost:3000"
cookie set

$ curl "http://localhost:3000/clear_cookie_foo"
cookie foo cleared
```

Console
```
document.cookie
"foo=bar"

document.cookie
""
```

