# [Node.js 全局对象](http://www.runoob.com/nodejs/nodejs-global-object.html)

## `__filename`

`__filename` 表示当前正在执行的脚本的文件名。它将输出文件所在位置的绝对路径，且和命令行参数所指定的文件名不一定相同。如果在模块中，返回的值是模块文件的路径。

```javascript
console.log(__filename);
```

## `__dirname`

`__dirname` 表示当前执行脚本所在的目录。

```javascript
console.log(__dirname);
```

## `setTimeout(cb, ms)`

`setTimeout(cb, ms)` 全局函数在指定的毫秒（ms）数后执行指定函数（cb）。`setTimeout()` 只执行一次指定函数，返回一个代表定时器的句柄值。

```javascript
function printHello() {
    console.log('Hello World');
}

setTimeout(printHello, 2000);
```

## `clearTimeout(t)`

`clearTimeout(t)` 全局函数用于停止一个之前通过 `setTimeout()` 创建的定时器。参数 `t` 是通过 `setTimeout()` 函数创建的定时器。

```javascript
function printHello() {
    console.log('Hello World');
}

var t = setTimeout(printHello, 2000);

clearTimeout(t);
```

## `setInterval(cb, ms)`

`setInterval(cb, ms)` 全局函数在指定的毫秒（ms）数后执行指定函数（cb）。返回一个代表定时器的句柄值。可以使用 `clearInterval(t)` 函数来清除定时器。`setInterval()` 方法会不停地调用函数，直到 `clearInterval()` 被调用或窗口被关闭。

```javascript
function printHello() {
    console.log('Hello World');
}

setInterval(printHello, 2000);
```

## `console`

```javascript
console.log('Apple, Banana');
console.log('Apple%dBanana');
console.log('Apple%dBanana', 1991);
```

输出结果：
```
$ node main.js
Apple, Banana
Apple%dBanana
Apple1991Banana
```

```javascript
console.trace();
```

输出结果：
```
$ node main.js
Trace
    at Object.<anonymous> (main.js:1:71)
    at Module._compile (module.js:635:30)
    at Object.Module._extensions..js (module.js:646:10)
    at Module.load (module.js:554:32)
    at tryModuleLoad (module.js:497:12)
    at Function.Module._load (module.js:489:3)
    at Function.Module.runMain (module.js:676:10)
    at startup (bootstrap_node.js:187:16)
    at bootstrap_node.js:608:3
```

```javascript
console.log('start');

console.time('time');

var counter = 10;
console.log('counter: %d', counter);

console.timeEnd('time');

console.log('end');
```

输出结果：
```
$ node main.js
start
counter: 10
time: 0.291ms
end
```

## `process`

```javascript
process.on('exit', function (code) {
    setTimeout(function () {
        console.log('该代码不会执行');
    }, 0);

    console.log('退出码：' + code);
});

console.log('程序执行结束');
```

输出结果：
```
$ node main.js
程序执行结束
退出码：0
```


```javascript
process.stdout.write('Hello, world!\n');

process.argv.forEach(function (val, index, array) {
    console.log(index + ': ' + val);
});

console.log(process.execPath);

console.log(process.platform);
```

输出结果：
```
$ node main.js
Hello, world!
0: /Users/yuhuihuang/node/node-v8.9.3-darwin-x64/bin/node
1: /Users/yuhuihuang/Workspaces/Node/main.js
/Users/yuhuihuang/node/node-v8.9.3-darwin-x64/bin/node
darwin
```

```javascript
console.log(process.cwd());

console.log(process.version);

console.log(process.memoryUsage());
```

输出结果：
```
$ node main.js
/Users/yuhuihuang/Workspaces/Node
v8.9.3
{ rss: 21274624,
  heapTotal: 7708672,
  heapUsed: 4439936,
  external: 8224 }
```