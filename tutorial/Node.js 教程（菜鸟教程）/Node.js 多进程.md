# [Node.js 多进程](http://www.runoob.com/nodejs/nodejs-process.html)

## `exec()` 方法

`./support.js`
```javascript
console.log('进程 ' + process.argv[2] + ' 执行');
```

`./master.js`
```javascript
const child_process = require('child_process');

for (var i = 0; i < 3; i++) {
   var workerProcess = child_process.exec('node support.js ' + i, function (error, stdout, stderr) {
      if (error) {
         console.log(error.stack);
         console.log('Error code: ' + error.code);
         console.log('Signal received: ' + error.signal);
      }
      console.log('stdout: ' + stdout);
      console.log('stderr: ' + stderr);
   });
   workerProcess.on('exit', function (code) {
      console.log('子进程退出，退出码：' + code);

   });
}
```

输出结果：
```
$ node master.js
子进程退出，退出码：0
stdout: 进程 0 执行

stderr:
子进程退出，退出码：0
stdout: 进程 1 执行

stderr:
子进程退出，退出码：0
stdout: 进程 2 执行

stderr:
```

## `spawn()` 方法

`./support.js`
```javascript
console.log('进程 ' + process.argv[2] + ' 执行');
```

`./master.js`
```javascript
const child_process = require('child_process');

for (var i = 0; i < 3; i++) {
   var workerProcess = child_process.spawn('node', ['support.js', i]);

   workerProcess.stdout.on('data', function (data) {
      console.log('stdout: ' + data);
   });

   workerProcess.stderr.on('data', function (data) {
      console.log('stderr: ' + data);
   });

   workerProcess.on('close', function (code) {
      console.log('子进程退出，退出码：' + code);
   });
}
```

输出结果：
```
$ node master.js
stdout: 进程 0 执行

子进程退出，退出码：0
stdout: 进程 1 执行

子进程退出，退出码：0
stdout: 进程 2 执行

子进程退出，退出码：0
```

## `fork` 方法

`./support.js`
```javascript
console.log('进程 ' + process.argv[2] + ' 执行');
```

`./master.js`
```javascript
const child_process = require('child_process');

for (var i = 0; i < 3; i++) {
   var workerProcess = child_process.fork('support.js', [i]);

   workerProcess.on('close', function (code) {
      console.log('子进程退出，退出码：' + code);
   });
}
```

输出结果：
```
$ node master.js
进程 0 执行
子进程退出，退出码：0
进程 1 执行
进程 2 执行
子进程退出，退出码：0
子进程退出，退出码：0
```