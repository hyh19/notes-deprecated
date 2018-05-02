# [Node.js 模块系统](http://www.runoob.com/nodejs/nodejs-module-system.html)

`./hello.js`
```javascript
exports.world = function () {
    console.log('Hello World');
};
```

`./main.js`
```javascript
var hello = require('./hello');
hello.world();
```

输出结果：
```
$ node main.js 
Hello World
```

`./hello`
```javascript
function Hello() {

    var name;

    this.setName = function (aName) {
        name = aName;
    };

    this.sayHello = function () {
        console.log('Hello ' + name);
    };
}

module.exports = Hello;
```

`./main.js`
```javascript
var Hello = require('./hello');

hello = new Hello();

hello.setName('Mike');

hello.sayHello();
```

输出结果：
```
$ node main.js
Hello Mike
```