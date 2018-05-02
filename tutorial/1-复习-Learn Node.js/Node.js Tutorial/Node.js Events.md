# [Node.js Events](https://www.w3schools.com/nodejs/nodejs_events.asp)

## Events in Node.js

```javascript
var fs = require('fs');

var readStream = fs.createReadStream('./sample.txt');
readStream.on('open', function () {
    console.log('The file is open');
});
```

```
$ node demo.js
The file is open
```

**API**

[fs.createReadStream(path[, options])](https://nodejs.org/dist/latest-v8.x/docs/api/fs.html#fs_fs_createreadstream_path_options)

[Class: fs.ReadStream](https://nodejs.org/dist/latest-v8.x/docs/api/fs.html#fs_class_fs_readstream)

[Event: 'open'](https://nodejs.org/dist/latest-v8.x/docs/api/fs.html#fs_event_open)

## The EventEmitter Object

```javascript
var events = require('events');
var eventEmitter = new events.EventEmitter();

var myEventHandler = function () {
    console.log('I hear a scream!');
}

eventEmitter.on('scream', myEventHandler);

eventEmitter.emit('scream');
```

```
$ node demo.js
I hear a scream!
```

**API**

[Events](https://nodejs.org/dist/latest-v8.x/docs/api/events.html)

[Class: EventEmitter](https://nodejs.org/dist/latest-v8.x/docs/api/events.html#events_class_eventemitter)