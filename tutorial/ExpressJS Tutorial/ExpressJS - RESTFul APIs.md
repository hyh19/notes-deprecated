# [ExpressJS - RESTFul APIs](https://www.tutorialspoint.com/expressjs/expressjs_restful_apis.htm)

```
$ npm install express --save
$ npm install body-parser --save
```

`./index.js`
```javascript
var express = require('express')
var app = express()

var bodyParser = require('body-parser')
app.use(bodyParser.json())
app.use(bodyParser.urlencoded({ extended: true }))

var movies = require('./movies.js')
app.use('/movies', movies)

app.listen(3000)
```

`./movies.js`
```javascript
var express = require('express')
var router = express.Router()
var movies = [
   {id: 101, name: "Fight Club", year: 1999, rating: 8.1},
   {id: 102, name: "Inception", year: 2010, rating: 8.7},
   {id: 103, name: "The Dark Knight", year: 2008, rating: 9},
   {id: 104, name: "12 Angry Men", year: 1957, rating: 8.9}
]

router.get('/', function (req, res) {
    res.json(movies)
})

router.get('/:id([0-9]{3,})', function (req, res) {
    var currMovie = movies.filter(function (movie) {
        if (movie.id == req.params.id) {
            return true
        }
    })
    if (currMovie.length == 1) {
        res.json(currMovie[0])
    } else {
        res.status(404)
        res.json({ message: 'Not Found' })
    }
})

router.post('/', function (req, res) {
    if (!req.body.name || 
        !req.body.year.toString().match(/^[0-9]{4}$/g) || 
        !req.body.rating.toString().match(/^[0-9]\.[0-9]$/g)) {
        res.status(400)
        res.json({ message: 'Bad Request' })
    } else {
        var newId = movies[movies.length-1].id + 1
        movies.push({
            id: newId,
            name: req.body.name,
            year: req.body.year,
            rating: req.body.rating
        })
        res.json({ message: 'New movie created', location: '/movies/' + newId })
    }
})

router.put('/:id', function (req, res) {
    if (!req.body.name || 
        !req.body.year.toString().match(/^[0-9]{4}$/g) || 
        !req.body.rating.toString().match(/^[0-9]\.[0-9]$/g) || 
        !req.params.id.toString().match(/^[0-9]{3,}$/g)) {
        res.status(400)
        res.json({ message: 'Bad Request' })
    } else {
        var updateIndex = movies.map(function (movie) {
            return movie.id
        }).indexOf(parseInt(req.params.id))

        if (updateIndex === -1) {
            movies.push({
                id: req.params.id,
                name: req.body.name,
                year: req.body.year,
                rating: req.body.rating
            })
            res.json({ message: 'New movie created', location: '/movies/' + req.params.id })
        } else {
            movies[updateIndex] = {
                id: req.params.id,
                name: req.body.name,
                year: req.body.year,
                rating: req.body.rating
            }
            res.json({ message: 'Movie id ' + req.params.id + ' updated.', location: '/movies/' + req.params.id })
        }
    }
})

router.delete('/:id', function (req, res) {
    var removeIndex = movies.map(function (movie) {
        return movie.id
    }).indexOf(parseInt(req.params.id))
    if (removeIndex === -1) {
        res.json({ message: 'Not Found' })
    } else {
        movies.splice(removeIndex, 1)
        res.send({ message: 'Movie id ' + req.params.id + ' removed.' })
    }
})

module.exports = router
```

Client
```
$ curl -i -H "Accept: application/json" -H "Content-Type: application/json" -X GET localhost:3000/movies
HTTP/1.1 200 OK
X-Powered-By: Express
Content-Type: application/json; charset=utf-8
Content-Length: 229
ETag: W/"e5-M+2qjV23q7Bhw3CBsyyCpATfcqY"
Date: Fri, 22 Dec 2017 03:52:45 GMT
Connection: keep-alive

[{"id":101,"name":"Fight Club","year":1999,"rating":8.1},{"id":102,"name":"Inception","year":2010,"rating":8.7},{"id":103,"name":"The Dark Knight","year":2008,"rating":9},{"id":104,"name":"12 Angry Men","year":1957,"rating":8.9}]
```

```
$ curl -i -H "Accept: application/json" -H "Content-Type: application/json" -X GET localhost:3000/movies/101
HTTP/1.1 200 OK
X-Powered-By: Express
Content-Type: application/json; charset=utf-8
Content-Length: 55
ETag: W/"37-bm0qP+tppNpgWQRD3d/3I3YrTHA"
Date: Fri, 22 Dec 2017 03:53:17 GMT
Connection: keep-alive

{"id":101,"name":"Fight Club","year":1999,"rating":8.1}
```

```
$ curl -X POST --data "name=Toy%20story&year=1995&rating=8.5" http://localhost:3000/movies
{"message":"New movie created","location":"/movies/105"}
```

```
$ curl -i -H "Accept: application/json" -H "Content-Type: application/json" -X DELETE localhost:3000/movies/101
HTTP/1.1 200 OK
X-Powered-By: Express
Content-Type: application/json; charset=utf-8
Content-Length: 35
ETag: W/"23-SjTB1LDmExiJDNO0m7qXhxHlQDk"
Date: Fri, 22 Dec 2017 03:48:30 GMT
Connection: keep-alive

{"message":"Movie id 101 removed."}
```

