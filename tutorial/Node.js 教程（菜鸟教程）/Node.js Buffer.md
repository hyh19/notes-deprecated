# [Node.js Buffer](http://www.runoob.com/nodejs/nodejs-buffer.html)

## Buffer 与字符编码

```javascript
const buf = Buffer.from('runoob', 'ascii');

console.log(buf.toString('hex'));

console.log(buf.toString('base64'));
```

输出结果：
```
$ node main.js 
72756e6f6f62
cnVub29i
```

## 创建 Buffer 类

```javascript
const buf1 = Buffer.alloc(10);

const buf2 = Buffer.alloc(10, 1);

const buf3 = Buffer.allocUnsafe(10);

const buf4 = Buffer.from([1, 2, 3]);

const buf5 = Buffer.from('runoob');

const buf6 = Buffer.from('runoob', 'latin1');
```

## 写入缓冲区

```javascript
buf = Buffer.alloc(256);
len = buf.write('www.runoob.com');
console.log(len);
```

输出结果：
```
$ node main.js 
14
```

## 从缓冲区读取数据

```javascript
buf = Buffer.alloc(26);

for (var i = 0; i < 26; i++) {
    buf[i] = i + 97;
}

console.log(buf.toString('ascii'));
console.log(buf.toString('ascii', 0, 5));
console.log(buf.toString('utf8', 0, 5));
console.log(buf.toString(undefined, 0, 5));
```

输出结果：
```
$ node main.js 
abcdefghijklmnopqrstuvwxyz
abcde
abcde
abcde
```

## 将 Buffer 转换为 JSON 对象

```javas
var buf = Buffer.from('www.runoob.com');

var json = buf.toJSON(buf);

console.log(json);
```

输出结果：
```
$ node main.js 
{ type: 'Buffer',
  data: [ 119, 119, 119, 46, 114, 117, 110, 111, 111, 98, 46, 99, 111, 109 ] }
```

## 缓冲区合并

```javascript
var buf1 = Buffer.from('菜鸟教程 ');

var buf2 = Buffer.from('www.runoob.com');

var buf3 = Buffer.concat([buf1, buf2]);

console.log(buf3.toString());
```

输出结果：
```
$ node main.js 
菜鸟教程 www.runoob.com
```

## 缓冲区比较

```javascript
var buf1 = Buffer.from('ABC');

var buf2 = Buffer.from('ABCD');

var buf3 = Buffer.from('ABC');

var buf4 = Buffer.from('AB');

console.log(buf1.compare(buf2));

console.log(buf1.compare(buf3));

console.log(buf1.compare(buf4));
```

输出结果：
```
$ node main.js 
-1
0
1
```

## 拷贝缓冲区

```javascript
var buf1 = Buffer.from('abcdefghijkl');

var buf2 = Buffer.from('RUNOOB');

buf2.copy(buf1, 2);

console.log(buf1.toString());
```

输出结果：
```
$ node main.js 
abRUNOOBijkl
```

## 缓冲区裁剪

```javascript
var buf1 = Buffer.from('runoob');

var buf2 = buf1.slice(0, 2);

console.log(buf2.toString());
```

输出结果：
```
$ node main.js 
ru
```

## 缓冲区长度

```javascript
var buf = Buffer.from('www.runoob.com');

console.log(buf.length);
```

输出结果：
```
$ node main.js 
14
```

