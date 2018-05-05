# [ExpressJS - Hello World](https://www.tutorialspoint.com/expressjs/expressjs_hello_world.htm)

```javascript
const express = require('express')
const app = express()

app.get('/', function (req, res) {
    res.send('Hello World');
})

app.listen(3000)
```

Server
```
$ nodemon index.js
[nodemon] 1.13.3
[nodemon] to restart at any time, enter `rs`
[nodemon] watching: *.*
[nodemon] starting `node index.js`
```

Client
```
$ curl "http://localhost:3000"
Hello World
```
