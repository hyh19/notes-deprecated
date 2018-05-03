# [ExpressJS - Error Handling](https://www.tutorialspoint.com/expressjs/expressjs_error_handling.htm)

```javascript
var express = require('express')
var app = express()

app.get('/', function (req, res) {
    var err = new Error('Something went wrong')
    next(err)
})

app.use(function (err, req, res, next) {
    res.status(500)
    res.send('Oops, something went wrong.')
})

app.listen(3000)
```

```
$ curl localhost:3000
Oops, something went wrong.
```