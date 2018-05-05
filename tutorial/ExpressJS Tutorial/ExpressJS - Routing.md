# [ExpressJS - Routing](https://www.tutorialspoint.com/expressjs/expressjs_routing.htm)

## app.method(path, handler)

```javascript
var express = require('express')
var app = express()

app.get('/hello', function (req, res) {
    res.send("Hello World")
})

app.post('/hello', function (req, res) {
    res.send("You just called the post method at '/hello'!\n")
})

app.all('/test', function (req, res) {
    res.send("HTTP method doesn't have any effect on this route!");
});

app.listen(3000)
```

Client
```
$ curl -X GET "http://localhost:3000/hello"
Hello World

$ curl -X POST "http://localhost:3000/hello"
You just called the post method at '/hello'!

$ curl -X GET "http://localhost:3000/test"
HTTP method doesn't have any effect on this route!

$ curl -X POST "http://localhost:3000/test"
HTTP method doesn't have any effect on this route!

$ curl -X PUT "http://localhost:3000/test"
HTTP method doesn't have any effect on this route!

$ curl -X DELETE "http://localhost:3000/test"
HTTP method doesn't have any effect on this route!
```

## Routers

`./things.js`
```javascript
var express = require('express')
var router = express.Router()

router.get('/', function (req, res) {
    res.send('GET route on things.')
})

router.post('/', function (req, res) {
    res.send('POST route on things.')
})

module.exports = router
```

`./index.js`
```javascript
var express = require('express')
var app = express()

var things = require('./things.js')

app.use('/things', things)

app.listen(3000)
```

Client
```
$ curl -X GET "http://localhost:3000/things"
GET route on things.

$ curl -X POST "http://localhost:3000/things"
POST route on things.
```