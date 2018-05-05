# [ExpressJS - Static Files](https://www.tutorialspoint.com/expressjs/expressjs_static_files.htm)

`./views/static.pug`
```
html
    head
    body
        h3 Testing static file serving:
        img(src="/logo.png", alt="Testing Image")
```

`./index.js`
```javascript
var express = require('express')
var app = express()

app.set('view engine', 'pug')
app.set('views', './views')

app.use(express.static('public'))

app.get('/static', function (req, res) {
    res.render('static')
})

app.listen(3000)
```

```
$ tree public/
public/
└── logo.png
```

```
$ curl "http://localhost:3000/static"
<html><head></head><body><h3>Testing static file serving:</h3><img src="/logo.png" alt="Testing Image"/></body></html>
```

## Multiple Static Directories

`.views/static.pug`
```
html
    head
    body
        h3 Testing static file serving:
        img(src="/google.png", alt="Google")
        img(src="/baidu.png", alt="百度")
```

`./index.js`
```javascript
var express = require('express')
var app = express()

app.set('view engine', 'pug')
app.set('views', './views')

app.use(express.static('public'))
app.use(express.static('images'))

app.get('/static', function (req, res) {
    res.render('static')
})

app.listen(3000)
```

```
$ tree public/ images/
public/
└── google.png
images/
└── baidu.png
```

```
$ curl "http://localhost:3000/static"
<html><head></head><body><h3>Testing static file serving:</h3><img src="/google.png" alt="Google"/><img src="/baidu.png" alt="百度"/></body></html>
```

## Virtual Path Prefix

`./views/static.pug`
```
html
    head
    body
        h3 Testing static file serving:
        img(src="/google/logo_google.png", alt="Google")
        img(src="/baidu/logo_baidu.png", alt="百度")
```

`./index.js`
```javascript
var express = require('express')
var app = express()

app.set('view engine', 'pug')
app.set('views', './views')

app.use('/google', express.static('public'))
app.use('/baidu', express.static('images'))

app.get('/static', function (req, res) {
    res.render('static')
})

app.listen(3000)
```

```
$ tree public/ images/
public/
└── logo_google.png
images/
└── logo_baidu.png
```

```
$ curl "http://localhost:3000/static"
<html><head></head><body><h3>Testing static file serving:</h3><img src="/google/logo_google.png" alt="Google"/><img src="/baidu/logo_baidu.png" alt="百度"/></body></html>
```