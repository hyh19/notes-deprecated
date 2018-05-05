# [Node.js 工具模块](http://www.runoob.com/nodejs/nodejs-utitlity-module.html)

## [Node.js OS 模块](http://www.runoob.com/nodejs/nodejs-os-module.html)

```javascript
var os = require('os');

console.log('endianness: ' + os.endianness());
console.log('type: ' + os.type());
console.log('platform: ' + os.platform());
console.log('total memory: ' + os.totalmem() + ' bytes');
console.log('free memory: ' + os.freemem() + ' bytes');
```

输出结果：
```
$ node main.js
endianness: LE
type: Windows_NT
platform: win32
total memory: 8529219584 bytes
free memory: 2636869632 bytes
```

## [Node.js Path 模块](http://www.runoob.com/nodejs/nodejs-path-module.html)

```javascript
var path = require('path');

console.log('normalization: ' + path.normalize('/dir1/dir2/dir3/file/..'));
console.log('joint path: ' + path.join('/dir1', 'dir2', 'dir3', 'file', '..'));
console.log('resolve: ' + path.resolve('main.js'));
console.log('ext name: ' + path.extname('main.js'));
```

输出结果：
```
$ node main.js
normalization: \dir1\dir2\dir3
joint path: \dir1\dir2\dir3
resolve: C:\cygwin64\home\huangyh2\demo\main.js
ext name: .js
```

## [Node.js Net 模块](http://www.runoob.com/nodejs/nodejs-net-module.html)

`./server.js`
```javascript
var net = require('net');

var server = net.createServer(function (connection) {
    console.log('有客户端连接进来');
    connection.on('end', function () {
        console.log('客户端关闭连接');
    });
    connection.write('Hello, Node.js!\r\n');
    connection.pipe(connection)
});

server.listen(8080, function () {
    console.log('服务器正在监听');
});
```

`./client.js`
```javascript
var net = require('net');

var client = net.connect({port: 8080}, function () {
    console.log('连接到服务器');
});

client.on('data', function (data) {
    console.log(data.toString());
    client.end();
});

client.on('end', function () {
    console.log('断开与服务器的连接');
});
```

输出结果：
```
$ node server.js
服务器正在监听
有客户端连接进来
客户端关闭连接
```

```
$ node client.js
连接到服务器
Hello, Node.js!

断开与服务器的连接
```

## [Node.js DNS 模块](http://www.runoob.com/nodejs/nodejs-dns-module.html)

```javascript
var dns = require('dns');

dns.lookup('www.github.com', function (err, address, family) {
    console.log(address);

    dns.reverse(address, function (err, hostnames) {
        if (err) {
            console.log(err.stack);
        }
        console.log(JSON.stringify(hostnames));
    });
});
```

输出结果：
```
$ node main.js
192.30.255.112
["lb-192-30-255-112-sea.github.com"]
```

## [Node.js Domain 模块](http://www.runoob.com/nodejs/nodejs-domain-module.html)

```javascript
var EventEmitter = require('events').EventEmitter;
var domain = require('domain');

var emitter1 = new EventEmitter();
var domain1 = domain.create();

domain1.on('error', function (err) {
    console.log('domain1 处理这个错误：' + err.message);
});

domain1.add(emitter1);

emitter1.on('error', function (err) {
    console.log('监听器处理这个错误：' + err.message);
});

emitter1.emit('error', new Error('通过监听器来处理'));

emitter1.removeAllListeners('error');

emitter1.emit('error', new Error('通过 domain1 来处理'));

var domain2 = domain.create();

domain2.on('error', function (err) {
    console.log('domain2 处理这个错误：' + err.message);
});

domain2.run(function () {
    var emitter2 = new EventEmitter();
    emitter2.emit('error', new Error('通过 domain2 处理这个错误'))
});

domain1.remove(emitter1);
emitter1.emit('error', new Error('转换为异常，系统将崩溃'));
```

输出结果：
```
$ node main.js
监听器处理这个错误：通过监听器来处理
domain1 处理这个错误：通过 domain1 来处理
domain2 处理这个错误：通过 domain2 处理这个错误
events.js:183
      throw er; // Unhandled 'error' event
      ^

Error: 转换为异常，系统将崩溃
    at Object.<anonymous> (C:\cygwin64\home\huangyh2\demo\main.js:35:24)
    at Module._compile (module.js:635:30)
    at Object.Module._extensions..js (module.js:646:10)
    at Module.load (module.js:554:32)
    at tryModuleLoad (module.js:497:12)
    at Function.Module._load (module.js:489:3)
    at Function.Module.runMain (module.js:676:10)
    at startup (bootstrap_node.js:187:16)
    at bootstrap_node.js:608:3
```