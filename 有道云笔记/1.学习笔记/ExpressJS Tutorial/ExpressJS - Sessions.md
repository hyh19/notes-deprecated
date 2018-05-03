# [ExpressJS - Sessions](https://www.tutorialspoint.com/expressjs/expressjs_sessions.htm)

```
$ npm install --save cookie-parser
$ npm install --save express-session
```

```javascript
var express = require('express')
var app = express()

var cookieParser = require('cookie-parser')
app.use(cookieParser())

var session = require('express-session')
app.use(session({ secret: "Shh, its a serect!" }))

app.get('/', function (req, res) {
    if (req.session.page_views) {
        req.session.page_views++
        res.send("You visited this page " + req.session.page_views + " times")
    } else {
        req.session.page_views = 1
        res.send("Welcome to this page for the first time!")
    }
})

app.listen(3000)
```

http://localhost:3000/

**API**

https://www.npmjs.com/package/express-session