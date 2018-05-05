# [ExpressJS - URL Building](https://www.tutorialspoint.com/expressjs/expressjs_url_building.htm)

```javascript
var express = require('express')
var app = express()

app.get('/:id', function (req, res) {
    res.send('The id you specified is ' + req.params.id)
})

app.listen(3000)
```

Client
```
$ curl "http://localhost:3000/123"
The id you specified is 123
```

```javascript
var express = require('express')
var app = express()

app.get('/things/:name/:id', function (req, res) {
    res.send('id: ' + req.params.id + ' and name: ' + req.params.name)
})

app.listen(3000)
```

Client
```
$ curl "http://localhost:3000/things/tutorialspoint/12345"
id: 12345 and name: tutorialspoint
```

## Pattern Matched Routes

```javascript
var express = require('express')
var app = express()

app.get('/things/:id([0-9]{5})', function (req, res) {
    res.send('id: ' + req.params.id)
})

app.get('*', function (req, res) {
    res.send('Sorry, this is an invalid URL.')
})

app.listen(3000)
```

Client
```
$ curl "http://localhost:3000/things/14554"
id: 14554

$ curl "http://localhost:3000/things/145"
Sorry, this is an invalid URL.
```
