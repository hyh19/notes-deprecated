# [ExpressJS - Form Data](https://www.tutorialspoint.com/expressjs/expressjs_form_data.htm)

```
$ npm install --save body-parser multer
```

`./views/form.pug`
```
html
    head
        title Form Tester
    body
        form(action="/form", method="POST")
            div
                label(for="say") Say: 
                input(name="say" value="Hi")
            br
            div
                label(for="to") To: 
                input(name="to" value="Express forms")
            br
            button(type="submit") Send my greetings
```

`./index.js`
```
var express = require('express')
var bodyParser = require('body-parser')
var app = express()

app.set('view engine', 'pug')
app.set('views', './views')

app.use(bodyParser.urlencoded({ extended: true }))

app.get('/form', function (req, res) {
    res.render('form')
})

app.post('/form', function (req, res) {
    console.log(req.body)
    res.send('Received your request!')
})

app.listen(3000)
```

```
$ curl "http://localhost:3000/form"
<html><head><title>Form Tester</title></head><body><form action="/form" method="POST"><div><label for="say">Say: </label><input name="say" value="Hi"/></div><br/><div><label for="to">To: </label><input name="to" value="Express forms"/></div><br/><button type="submit">Send my greetings</button></form></body></html>
```

```
[nodemon] starting `node index.js`
{ say: 'Hi', to: 'Express forms' }
```
