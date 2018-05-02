# [Node.js EventEmitter](http://www.runoob.com/nodejs/nodejs-event.html)

## EventEmitter 类

```javascript
var EventEmitter = require('events').EventEmitter;

var event = new EventEmitter();

event.on('some_event', function () {
    console.log('some_event 事件触发');
});

setTimeout(function () {
    event.emit('some_event');
}, 2500);
```

```javascript
var events = require('events');

var emitter = new events.EventEmitter();

emitter.on('someEvent', function (arg1, arg2) {
    console.log('Listener 1', arg1, arg2);
});

emitter.on('someEvent', function (arg1, arg2) {
    console.log('Listener 2', arg1, arg2);
});

emitter.emit('someEvent', 'Apple', 'Banana');
```

```javascript
var events = require('events');
var eventEmitter = new events.EventEmitter();

var listener1 = function () {
    console.log('监听器 listener1 执行');
};

var listener2 = function () {
    console.log('监听器 listener2 执行');
};

eventEmitter.addListener('connection', listener1);

eventEmitter.on('connection', listener2);

var listenerCount = events.EventEmitter.listenerCount(eventEmitter, 'connection');
console.log(listenerCount + ' 个监听器监听连接事件');

eventEmitter.emit('connection');

eventEmitter.removeListener('connection', listener1);
console.log('监听器 listener1 被移除');

listenerCount = events.EventEmitter.listenerCount(eventEmitter, 'connection');
console.log(listenerCount + ' 个监听器监听连接事件');

eventEmitter.emit('connection');

console.log('程序执行完毕');
```


```javascript
var events = require('events');
var emitter = new events.EventEmitter();
emitter.emit('error');
```

