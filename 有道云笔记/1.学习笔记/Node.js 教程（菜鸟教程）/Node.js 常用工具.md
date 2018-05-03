# [Node.js 常用工具](http://www.runoob.com/nodejs/nodejs-util.html)

## `util.inherits`

```javascript
var util = require('util');

function Base() {
    this.name = 'Base';
    this.sayHello = function () {
        console.log('Hello ' + this.name);
    };
}

Base.prototype.showName = function () {
    console.log(this.name);
};

function Sub() {
    this.name = 'Sub';
}

util.inherits(Sub, Base);

var objBase = new Base();
objBase.showName();
objBase.sayHello();
console.log(objBase);

var objSub = new Sub();
objSub.showName();
// objSub.sayHello();
console.log(objSub);
```

输出结果：
```
$ node demo.js
Base
Hello Base
Base { name: 'Base', sayHello: [Function] }
Sub
Sub { name: 'Sub' }
```

## `util.inspect`

```javascript
var util = require('util');

function Person() {
    this.name = 'Mike';
    this.toString = function () {
        return this.name;
    };
}

var obj = new Person();
console.log(util.inspect(obj));
console.log(util.inspect(obj, true));
```

输出结果：
```
$ node demo.js
Person { name: 'Mike', toString: [Function] }
Person {
  name: 'Mike',
  toString:
   { [Function]
     [length]: 0,
     [name]: '',
     [arguments]: null,
     [caller]: null,
     [prototype]: { [constructor]: [Circular] } } }
```

## `util.isArray(object)`

```javascript
var util = require('util');

console.log(util.isArray([]));
console.log(util.isArray(new Array()));
console.log(util.isArray({}));
```

输出结果：
```
$ node demo.js
true
true
false
```

## `util.isRegExp(object)`

```javascript
var util = require('util');

console.log( util.isRegExp(/some regexp/) );
console.log( util.isRegExp(new RegExp('another regexp')) );
console.log( util.isRegExp({}) );
```

输出结果：
```
$ node demo.js
true
true
false
```

## `util.isDate(object)`

```javascript
var util = require('util');

console.log( util.isDate(new Date()) );
console.log( util.isDate(Date()) );
console.log( util.isDate({}) );
```

输出结果：
```
$ node demo.js
true
false
false
```

## `util.isError(object)`

```javascript
var util = require('util');

console.log( util.isError(new Error()) );
console.log( util.isError(new TypeError()) );
console.log( util.isError({ name: 'Error', message: 'an error occured' }) );
```

输出结果：
```
$ node demo.js
true
true
false
```

**API**

[Util](https://nodejs.org/dist/latest-v8.x/docs/api/util.html)