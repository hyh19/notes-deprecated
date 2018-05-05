[TOC]

# 第 13 章 JavaScript 语法详解

## 13.1 JavaScript 简介

### 13.1.1 运行 JavaScript

在 HTML 页面中嵌入执行 JavaScript 代码有两种方式。
- 使用 `javascript:` 前缀构建执行 JavaScript 代码的 URL。
- 使用 `<script/>` 元素来包含 JavaScript 代码。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<a href="javascript:alert('Hello World');">Hello World</a>
<script>
    alert("Hello JavaScript");
</script>
</body>
</html>
```

### 13.1.2 导入 JavaScript 文件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<script src="test.js"></script>
</body>
</html>
```

## 13.2 数据类型和变量

### 13.2.1 定义变量的方式

JavaScript 是弱类型脚本语言，使用变量之前，可以无须定义，想使用某个变量时直接使用即可。归纳起来，JavaScript 支持两种方式来引入变量。
- 隐式定义：直接给变量赋值。
- 显式定义：使用 `var` 关键字定义变量。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<script type="text/javascript">
    a = "Hello World";
    alert(a);
</script>
</body>
</html>
```

显式声明方式是采用 `var` 关键字声明变量，声明时变量可以没有初始值，声明的变量数据类型是不确定的。当第一次给变量赋值时，变量的数据类型才确定下来，而且使用过程中变量的数据类型也可随意改变。
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<script type="text/javascript">
    var a;
    a = true;
    alert(a);
</script>
</body>
</html>
```

### 13.2.2 类型转换

JavaScript 支持自动类型转换
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<script type="text/javascript">
    var a = "3.145";
    var b = a - 2;
    var c = a + 2;
    alert(b + "\n" + c);
</script>
</body>
</html>
```

输出结果：
```
1.145
3.1452
```

JavaScript 提供了如下几个函数来执行强制类型转换。
- `toString()`：将布尔值、数值等转换成字符串。
- `parseInt()`：将字符串、布尔值等转换成整数。
- `parseFloat()`：将字符串、布尔值等转换成浮点数。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<script type="text/javascript">
    var a = "3.145";
    var b = a + 2;
    var c = parseFloat(a) + 2;
    alert(b + "\n" + c);
</script>
</body>
</html>
```

当使用 `parseInt()` 或 `parseFloat()` 将各种类型的变量转换成数值类型时，结果如下：
- 字符串值：如果字符串是一个数值字符串，则可以转换成一个数值，否则将转换成 `NaN`。
- `undefined`、`null`、布尔值及其他对象：一律转换成 `NaN`。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<script type="text/javascript">
    document.writeln(parseInt("Hello World"));
    document.writeln(parseInt(undefined));
    document.writeln(parseInt(null));
    document.writeln(parseInt(true));
</script>
</body>
</html>
```

当使用 `toString()` 函数将各种类型的值向字符串转换时，结果全部是 `object`。

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/toString

【注：在 API 文档搜索 `toString` 查看详情】

### 13.2.3 变量

根据变量定义的范围不同，变量有全局变量和局部变量之分。直接定义的变量是全局变量，全局变量可以被所有的脚本访问；在函数里定义的变量称为局部变量，局部变量只在函数内有效。如果全局变量和局部变量使用相同的变量名，则局部变量将覆盖全局变量。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<script type="text/javascript">
    var test = "Global Variable";
    function checkScope() {
        var test = "Local Variable";
        document.writeln(test);
    }
    checkScope();
</script>
</body>
</html>
```

与 Java、C 等语言不同的是，JavaScript 的变量没有块范围。
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<script type="text/javascript">
    function test(o) {
        var i = 0;
        if (typeof o == "object") {
            var j = 5; // 变量 j 的作用范围是整个函数内，而不仅仅是在 if 块内。
            for (var k = 0; k < 10; k++) { // k 的作用范围是整个函数内，而不是循环体内。
                document.write(k);
            }
        }
        document.write(k + "\n" + j);
    }
    test(document);
</script>
</body>
</html>
```

输出结果：
```
012345678910 5
```

因为 JavaScript 的变量没有块范围，有时可能出现一些非常奇怪的结果。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<script type="text/javascript">
    var scope = "Global Variable";
    function test() {
        // 由于 scope 的作用域是整个函数，此时还没有被赋值，所以其值是 undefined。
        document.writeln(scope + "<br/>");
        // 覆盖全局变量的值
        var scope = "Local Variable";
        document.writeln(scope + "<br/>");
    }
    test();
</script>
</body>
</html>
```

输出结果：
```
undefined
Local Variable
```

定义变量用 `var` 和不用 `var` 的差异：
- 如果使用 `var` 定义变量，那么程序会强制定义一个新变量。
- 如果没有使用 `var` 定义变量，系统会优先在当前上下文中搜索是否存在该变量。只有在该变量不存在的前提下，系统才会重新定义一个新变量。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<script type="text/javascript">
    var scope = "Global Variable";
    function test() {
        // 由于后面的变量 scope 没有用 var 来声明，所以编译器优先搜索上下文已存在的变量，在这里首先找到的是全局变量。
        document.writeln(scope + "<br/>");
        // 不是定义一个新变量，而是给全局变量赋一个新的值。
        scope = "Local Variable";
        document.writeln(scope + "<br/>");
    }
    test();
</script>
</body>
</html>
```

输出结果：
```
Global Variable
Local Variable
```

复习到这

## 13.3 基本数据类型

JavaScript 的基本数据类型有如下 5 个：
- 数值类型：包含整数或浮点数。
- 布尔类型：只有 `true` 或 `false` 两个值。
- 字符串类型：字符串变量必须用引号括起来，引号可以是单引号，也可以是双引号。
- `undefined` 类型：专门用来确定一个已经创建但是没有初值的变量。
- `null` 类型：用于表明某个变量的值为空。

### 13.3.1 数值类型

科学计数法
```javascript
// 显式声明变量
var a = 5E2;
var b = 1.23e-3;
console.log("a = " + a);
console.log("b = " + b);
```

输出结果：
```bash
a = 500
b = 0.00123
```

如果数值只有小数部分，则可以省略整数部分的 0，但小数点不能省略。
```javascript
// 隐式声明变量
a = 3.12e1;
b = 45.0;
c = .34e4;
d = .24e-2;
console.log("a = " + a);
console.log("b = " + b);
console.log("c = " + c);
console.log("d = " + d);
```

输出结果：
```bash
a = 31.2
b = 45
c = 3400
d = 0.0024
```

数值直接量不要以 0 开头。因为 JavaScript 不仅支持十进制数，还支持其他进制数。八进制数以 0 开头，十六进制数以 `0x` 或者 `0X` 开头。
```javascript
var a = 0x13;
var b = 014;
console.log("a = " + a);
console.log("b = " + b);
```

输出结果：
```bash
a = 19
b = 12
```

当数值变量的值超出了其表数范围时，将出现两个特殊值：`Infinity`（正无穷大）和 `−Infinity`（负无穷大）。前者表示数值大于数值类型的最大正数，后者表示数值小于数值类型的最小负数。
```javascript
var x = 1.7976931348623157e308;
x = x + 1e292;
console.log(x);

x = -1.7976931348623157e308;
x = x - 1e292;
console.log(x);
```

输出结果：
```bash
Infinity
-Infinity
```

`Infinity` 和 `−Infinity` 相互运算的结果：
```javascript
a = Number.POSITIVE_INFINITY;
b = Number.NEGATIVE_INFINITY;
console.log("a + a = " + (a + a));
console.log("a - a = " + (a - a));
console.log("b + b = " + (b + b));
console.log("b - b = " + (b - b));
console.log("a + b = " + (a + b));
console.log("a - b = " + (a - b));
console.log("b + a = " + (b + a));
console.log("b - a = " + (b - a));
```

输出结果：
```bash
a + a = Infinity
a - a = NaN
b + b = -Infinity
b - b = NaN
a + b = NaN
a - b = Infinity
b + a = NaN
b - a = -Infinity
```

`Infinity`、`−Infinity` 和一般数值运算的结果：
```javascript
a = Number.POSITIVE_INFINITY;
b = Number.NEGATIVE_INFINITY;
console.log("a + 10 = " + (a + 10));
console.log("a - 10 = " + (a - 10));
console.log("b + 10 = " + (b + 10));
console.log("b - 10 = " + (b - 10));
```

输出结果：
```bash
a + 10 = Infinity
a - 10 = Infinity
b + 10 = -Infinity
b - 10 = -Infinity
```

`Infinity`、`−Infinity` 和 `NaN` 运算的结果：
```javascript
a = Number.POSITIVE_INFINITY;
b = Number.NEGATIVE_INFINITY;
console.log("a + NaN = " + (a + NaN));
console.log("a - NaN = " + (a - NaN));
console.log("b + NaN = " + (b + NaN));
console.log("b - NaN = " + (b - NaN));
```

输出结果：
```bash
a + NaN = NaN
a - NaN = NaN
b + NaN = NaN
b - NaN = NaN
```

**注意：如果算术表达式中有 `NaN` 的数值变量，则整个算术表达式的值为 `NaN`。**
```javascript
var a = Number.POSITIVE_INFINITY;
var b = Number.NEGATIVE_INFINITY;
var c = 10;
console.log("a + NaN = " + (a + NaN));
console.log("b + NaN = " + (b + NaN));
console.log("c + NaN = " + (c + NaN));
```

输出结果：
```bash
a + NaN = NaN
b + NaN = NaN
c + NaN = NaN
```

`Infinity` 和 `−Infinity` 都可以执行**比较运算**：两个 `Infinity` 总是相等的，而两个 `−Infinity` 也总是相等的。
```javascript
var a = 3e30000;
var b = 5e20000;
console.log("a = " + a);
console.log("b = " + b);
console.log(a == b);

a = -3e30000;
b = -5e20000;
console.log("a = " + a);
console.log("b = " + b);
console.log(a == b);
```

输出结果：
```bash
a = Infinity
b = Infinity
true
a = -Infinity
b = -Infinity
true
```

`NaN` 不会与任何数值相等，即使是 `NaN == NaN` 也会返回 `false`。
```javascript
console.log(NaN == 76);
console.log(NaN == Infinity);
console.log(NaN == -Infinity);
console.log(NaN == NaN);
```

输出结果：
```bash
false
false
false
false
```

JavaScript 中的算术运算允许除数为 0（除数和被除数也可同时为 0，得到结果为 `NaN`），正数除以零的结果就是 `Infinity`，负数除以零的结果就是 `−Infinity`。
```javascript
console.log(0/0);
console.log(13/0);
console.log(-13/0);
```

输出结果：
```bash
NaN
Infinity
-Infinity
```

JavaScript 专门提供了 `isNaN()` 函数来判断某个变量是否为 `NaN`。
```javascript
var x = NaN;
console.log(isNaN(x));

x = 123;
console.log(isNaN(x));
```

输出结果：
```bash
true
false
```

关于浮点型数，必须注意其精度丢失的问题。
```javascript
var a = .3333;
var b = a * 5;
console.log(b);
```

输出结果：
```bash
1.6664999999999999
```

这种由于浮点数计算产生的问题，在很多语言中都会出现。对于浮点数值的比较，尽量不要直接比较，例如直接比较 `b` 是否等于 1.6665，将返回不相等。为了得到 1.6665 与 `b` 相等的结果，推荐使用差值比较法——判断两个浮点型变量的差值，只要差值小于一个足够小的数即可认为相等。

**`Number` 类的常量与特殊值的对应**
常量 | 特殊值
---|---
[`Number.MAX_VALUE`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/MAX_VALUE) | 数值变量允许的最大值
[`Number.MIN_VALUE`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/MIN_VALUE) | 数值变量允许的最小正值
[`Number.POSITIVE_INFINITY`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/POSITIVE_INFINITY) | [`Infinity`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Infinity)
[`Number.NEGATIVE_INFINITY`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/NEGATIVE_INFINITY) | `-Infinity`
[`Number.NaN`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/NaN) | [`NaN`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/NaN)

```javascript
console.log(Number.MAX_VALUE);
console.log(Number.MIN_VALUE);
console.log(Number.POSITIVE_INFINITY);
console.log(Number.NEGATIVE_INFINITY);
console.log(Number.NaN);
```

输出结果：
```bash
1.7976931348623157e+308
5e-324
Infinity
-Infinity
NaN
```

## 13.8 函数

JavaScript 语言中的函数就是“一等公民”，它可以独立存在；而且 JavaScript 的函数完全可以作为一个类使用（而且它还是该类唯一的构造器）；与此同时，函数本身也是一个对象，函数本身是 `Function` 实例。

### 13.8.1 定义函数的 3 种方式

JavaScript 是弱类型语言，因此定义函数时，既不需要声明函数的返回值类型，也不需要声明函数的参数类型。

#### 13.8.1.1 定义命名函数

语法：
```javascript
function functionName(parameterList) {
    statements
}
```

```html
<script type="text/javascript">
    hello('Grace');
    function hello(name) {
        alert("Hi, " + name + "!");
    }
</script>
```

**注意：从上面程序中可以看出，在同一个 `<script.../>` 元素中时，JavaScript 允许先调用函数，然后再定义该函数；但在不同的 `<script.../>` 元素中时，必须先定义函数，再调用该函数。**

函数可以有返回值，也可以没有返回值。
```html
<script type="text/javascript">
    function hello(name) {
        if (typeof name == 'string') {
            return "Hi, " + name + "!";
        }
        return "The parameter 'name' must be a string."
    }
    alert(hello('Grace'));
</script>
```

#### 13.8.1.2 定义匿名函数

语法：
```javascript
function(parameterList) {
    statements
}
```

当通过这种语法格式定义了函数之后，实际上就是定义了一个函数对象（即 `Function` 实例），接下来可以将这个对象赋给另一个变量。

```html
<script type="text/javascript">
    var f = function(name) {
        document.writeln("Hi, " + name + "!");
    };
    f('Mike');
</script>
```

**注意：在函数定义语法的最后不要忘记紧跟分号（`;`）。**

#### 13.8.1.3 使用 `Function` 类匿名函数

JavaScript 提供了一个 `Function` 类，该类也可以用于定义函数。`Function` 类的构造器的参数个数不受限制，`Function` 可以接受一系列的字符串参数，其中最后一个字符串参数是函数的执行体，执行体的各语句以分号（`;`）隔开，而前面的各字符串参数则是函数的参数。

```html
<script type="text/javascript">
    var f = new Function('name', 
        "document.writeln('Hi, ' + name + '!');");
    f('Tom');
</script>
```

### 13.8.3 局部变量和局部函数

在函数里定义的变量称为局部变量，在函数外定义的变量则称为全局变量，如果局部变量和全局变量的名字相同，则局部变量会覆盖全局变量。局部变量只能在函数里访问，而全局变量可以在所有的函数里访问。

与此类似的概念是局部函数，局部变量在函数里定义，而局部函数也在函数里定义。

```html
<script type="text/javascript">
    function outer() {
        function inner1() {
            document.write("Inner 1 <br>");
        }

        function inner2() {
            document.write("Inner 2 <br>");
        }

        document.write("=== Before Inner === <br>");
        inner1();
        inner2();
        document.write("=== After Inner === <br>");
    }
    
    document.write("*** *** Before Outer *** *** <br>");
    outer();
    document.write("*** *** After Outer *** *** <br>");
</script>
```

### 13.8.4 函数、方法、对象和类

函数是 JavaScript 语言中的“一等公民”，当使用 JavaScript 定义一个函数后，实际上可以得到如下 4 项：
- 函数：就像 Java 的方法一样，这个函数可以被调用。
- 对象：定义一个函数时，系统也会创建一个对象，该对象是 `Function` 类的实例。
- 方法：定义一个函数时，该函数通常都会附加给某个对象，作为该对象的方法。
- 类：在定义函数的同时，也得到了一个与函数同名的类。

**函数作为一个对象**

```html
<script type="text/javascript">
    var hello = function(name) {
        return "Hi, " + name;
    };

    alert("Is 'hello' a Function instance? " + (hello instanceof Function));
    alert("Is 'hello' an Object instance? " + (hello instanceof Object));
    // 如果直接输出函数本身，将会输出其代码。
    alert(hello);
</script>
```

当我们定义一个函数后，有如下两种方式来调用函数：
- 直接调用函数：直接调用函数总是返回该函数体内最后一条 `return` 语句的返回值；如果该函数体内不包含 `return` 语句，则直接调用函数没有任何返回值。
- 使用 `new` 关键字调用函数：通过这种方式调用总有一个返回值，返回值就是一个 JavaScript 对象。

```html
<script type="text/javascript">
    var test = function(name) {
        return "Hi, " + name;
    };
    // 直接调用函数
    var rval = test('leegang');
    alert(rval);
    // 使用 new 关键字调用函数
    var obj = new test('leegang');
    alert(obj);
</script>
```

**函数作为一个类**

定义一个 `Person` 函数，也就是定义了一个 `Person` 类，该 `Person` 函数也会作为 `Person` 类唯一的构造器。
```html
<script type="text/javascript">
    function Person(name, age) {
        // 被 this 关键字修饰的变量不再是局部变量，它是该函数的实例属性。
        this.name = name;
        this.age = age;
        this.info = function() {
            document.writeln("My name is " + this.name + ".<br>");
            document.writeln("My age is " + this.age + ".<br>");
        };
    }
    var p = new Person('yeeku', 29);
    p.info();
</script>
```

**函数作为一个对象的方法**

JavaScript 定义的函数可以“附加”到某个对象上，作为该对象的方法。实际上，如果没有明确指定将函数“附加”到哪个对象上，该函数将“附加”到 `window` 对象上，作为 `window` 对象的方法。
```html
<script type="text/javascript">
    var hello = function(name) {
        document.write("Hi, " + name + ".<br>");
    }
    // 如果没有指定，默认附加到 window 对象上。
    window.hello("Mike");
</script>
```

```html
<script type="text/javascript">
    // 使用 {} 定义一个对象
    var p = {
        // 定义一个函数，赋值给属性 walk，该属性属于对象 p。
        walk: function() {
            for (var i=0; i<2; i++) {
                document.write("walking...");
            }
        }
    }
    p.walk();
</script>
```

### 13.8.5 函数的实例属性和类属性

函数中的变量有 3 种：
- 局部变量：在函数中以普通方式声明的变量，包括以 `var` 或不加任何前缀声明的变量。
- 实例属性：在函数中以 `this` 前缀修饰的变量。
- 类属性：在函数中以函数名前缀修饰的变量。

```html
<script type="text/javascript">
    function Person(national, age) {
        // 类属性
        Person.national = national;
        // 实例属性
        this.age = age;
        // 局部变量
        var lvar = 0;
    }

    document.writeln("=== The First Person ===<br>");
    var p1 = new Person('China', 29);
    document.writeln("p1 的 age 属性：" + p1.age + ".<br>");
    document.writeln("p1 的 national 属性：" + p1.national + ".<br>");
    document.writeln("通过 Person 访问静态 national 属性：" + Person.national + ".<br>");

    document.writeln("<hr>");

    document.writeln("=== The Second Person ===<br>");
    var p2 = new Person('USA', 30);
    document.writeln("p2 的 age 属性：" + p2.age + ".<br>");
    document.writeln("p2 的 national 属性：" + p2.national + ".<br>");
    document.writeln("通过 Person 访问静态 national 属性：" + Person.national + ".<br>");
</script>
```

给对象动态添加属性
```html
<script type="text/javascript">
    function Student(grade, subject) {
        this.grade = grade;
        Student.subject = subject;
    }

    s1 = new Student(5, 'Java');
    with(document) {
        writeln("s1 的 grade 属性：" + s1.grade + "<br>");
        writeln("s1 的 subject 属性：" + s1.subject + "<br>");
        writeln("Student 的 subject 属性：" + Student.subject + "<br>");
    }

    // 给对象动态添加属性
    s1.subject = 'Ruby';
    with(document) {
        writeln("=== 为 s1 的 subject 属性赋值后 ===<br>");
        writeln("s1 的 subject 属性：" + s1.subject + "<br>");
        writeln("Student 的 subject 属性：" + Student.subject + "<br>");
    }
</script>
```

**注意：如果直接定义一个全局变量，实际上这个全局变量会“附加”到 `window` 对象上，作为 `window` 对象的实例属性。因此，程序可以 `window` 对象作为调用者来访问这个全局变量。**

### 13.8.6 调用函数的 3 种方式

#### 13.8.6.1 直接调用函数

语法：
```
调用者.函数(参数 1, 参数 2...)
```

```html
window.alert("测试代码");
```

#### 13.8.6.2 以 `call()` 方法调用函数

语法：
```
函数引用.call(调用者, 参数 1, 参数 2, ...)
```

```html
<script type="text/javascript">
    var each = function(array, fn) {
        for (var index in array) {
            fn.call(null, index, array[index]);
        }
    };

    each([10, 20, 30], function(index, element) {
        document.write("索引为 " + index + " 的元素是：" + element + "<br>");
    });
</script>
```

直接调用函数与通过 `call()` 调用函数的关系：
```
调用者.函数(参数 1, 参数 2, ...) = 函数引用.call(调用者, 参数 1, 参数 2, ...)
```

#### 13.8.6.3 以 `apply()` 方法调用函数

语法：
```
函数引用.apply(调用者, [参数 1, 参数 2, ...])
```

`apply()` 与 `call()` 的区别如下：
- 通过 `call()` 调用函数时，必须在括号中详细地列出每个参数。
- 通过 `apply()` 动态地调用函数时，必须在括号中以数组形式传入参数。

```html
<script type="text/javascript">
    var fn1 = function(a, b) {
        document.writeln("a: " + a + ", b: " + b + "<br>");
    };
    fn1.call(window, 10, 20);
    // 使用数组传入参数
    fn1.apply(window, [30, 40]);

    var fn2 = function(aa, bb) {
        // arguments 表示调用函数时传入的所有参数，是一个数组。
        fn1.apply(this, arguments);
    };
    fn2(50, 60);
    // 逐个传入参数无效，必须是数组形式。
    fn1.apply(window, 70, 80);
</script>
```

### 13.8.7 函数的独立性

JavaScript 的函数是“一等公民”，它永远是独立的，函数永远不会从属于其他类、对象。

当使用匿名内嵌函数定义某个类的方法时，该内嵌函数一样是独立存在的，该函数也不是完全作为该类实例的附庸存在，这些内嵌函数也可以被分离出来独立使用，包括成为另一个对象的函数。

```html
<script type="text/javascript">
    function Person(name) {
        this.name = name;
        this.info = function() {
            document.writeln("name: " + this.name + "<br>");
        };
    }
    var p = new Person('Mike');
    p.info();

    var name = "Tom";
    p.info.call(window);
</script>
```

```html
<script type="text/javascript">
    function Dog(name, age, color) {
        this.name = name;
        this.age = age;
        this.color = color;
        this.info = function() {
            document.writeln("name: " + this.name + ", age: " + this.age + ", color: " + this.color + "<br>");
        };
    }

    var dog = new Dog("Lucky", 3, "White");
    dog.info();

    function Cat(name, age) {
        this.name = name;
        this.age = age;
    }

    var cat = new Cat('Kitty', 2);
    dog.info.call(cat);
</script>
```

## 13.9 函数的参数处理

JavaScript 的参数传递全部是采用***值传递***方式。

### 13.9.1 基本类型和复合类型的参数传递

对于基本类型参数，JavaScript 采用值传递方式，当通过实参调用函数时，传入函数里的并不是实参本身，而是实参的副本。
```html
<script type="text/javascript">
    function change(num) {
        num = 10;
        document.writeln("num: " + num + "<br>");
    }

    var x = 5;
    document.writeln(x + "<br>");
    change(x);
    document.writeln(x + "<br>");
</script>
```

对于复合类型的参数，实际上采用的依然是值传递方式。复合类型的变量本身并未持有对象本身，只是一个引用，该引用指向实际的 JavaScript 对象。
```html
<script type="text/javascript">
    function changeAge(person) {
        person.age = 10;
        document.write("[Change Age] age: " + person.age + "<br>");
        person = null;
    }

    var person = {age: 5};
    document.write("[Before] age: " + person.age + "<br>");
    changeAge(person);
    document.write("[After] age: " + person.age + "<br>");
    document.write("[After] person: " + person + "<br>");
</script>
```

### 13.9.2 空参数

JavaScript 会将没有传入实参的参数值自动设置为 `undefined` 值。
```html
<script type="text/javascript">
    function changeAge(person) {
        if (typeof person == 'object') {
            person.age = 10;
            document.write("age: " + person.age + "<br>");
        } else {
            document.write("type: " + typeof person + "<br>");
        }
    }

    changeAge();
</script>
```

**注意：JavaScript 没有所谓的函数“重载”，函数名就是函数的唯一标识。如果先后定义两个同名的函数，它们的形参列表并不相同，后面定义的函数会覆盖前面定义的函数。**
```html
<script type="text/javascript">
    function foo() {
        document.write("foo()");
    }
    function foo(name) {
        document.write("foo(name)");
    }

    foo();
</script>
```

### 13.9.3 参数类型

**注意：如果弱类型语言的函数需要接受参数，则应先判断参数类型，并判断参数是否包含了需要访问的属性、方法。只有当这些条件都满足时，程序才开始真正处理调用参数的属性、方法。**

```html
<script type="text/javascript">
    function changeAge(person) {
        if (typeof person == 'object' &&
            typeof person.age == 'number') {
            document.write("[Before] age: " + person.age + "<br>");
            person.age = 10;
            document.write("[After] age: " + person.age + "<br>");
        } else {
            document.write("[Error] type: " + typeof person + "<br>");
        }
    }

    changeAge();
    changeAge('abc');
    changeAge(true);
    p = {height: 175};
    changeAge(p);
    person = {age: 34};
    changeAge(person);
</script>
```

## 13.10 使用对象

### 13.10.1 面向对象的概念

**JavaScript 不是面向对象的程序设计语言**，面向对象设计的基本特征：**继承、多态**等都没有得到很好的实现。在纯粹的面向对象语言里，最基本的程序单位是类，类与类之间提供严格的继承关系。而 JavaScript 并没有提供规范的语法让开发者定义类，函数定义也不支持继承语法，所以，JavaScript 不是面向对象的程序设计语言，我们习惯上称 JavaScript 是**基于对象的脚本语言**。

所有类都是 `Object` 类的子类

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<script type="text/javascript">
    function Person(name) {
        this.name = name;
    }
    var p = new Person("Tom");
    document.writeln(p instanceof Person);
    document.writeln("<br>");
    document.writeln(p instanceof Object);
</script>
</body>
</html>
```

输出结果：
```
true 
true
```

## 13.10.2 对象和关联数组

JavaScript 中的对象本质上是一个关联数组，由一组 `key-value` 对组成。

当需要访问某个 JavaScript 对象的属性时，不仅可以使用 `obj.propName` 的形式，也可以采用 `obj[propName]` 的形式，有些时候甚至必须使用这种形式。
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<script type="text/javascript">
    function Person(name, age) {
        this.name = name;
        this.age = age;
        this.info = function () {
            document.writeln("name: " + this.name + ", age: " + this.age);
        };
    }

    var p = new Person("Tom", 30);
    for (propName in p) {
        // 如果采用 p.propName 的形式，程序试图直接访问该对象内名字叫 "propName" 的属性，但该属性并不存在。
        document.writeln(propName + ": " + p[propName] + "<br>");
    }
</script>
</body>
</html>
```

输出结果：
```
name: Tom
age: 30
info: function () { document.writeln("name: " + this.name + ", age: " + this.age); }
```

### 13.10.3 继承和 `prototype`

JavaScript 是一种动态语言，它允许自由地为对象增加属性和方法，当程序为对象的某个不存在的属性赋值时，即可认为是为该对象增加属性。
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<script type="text/javascript">
    var p = {};
    p.age = 30;
    p.info = function() {
        document.writeln("age: " + this.age);
    };
    p.info();
</script>
</body>
</html>
```

输出结果：
```
age: 30
```

当定义函数时，函数中以 `this` 修饰的变量是实例属性【注：用该函数创建的对象实例的属性】，如果某个属性值是函数时，即可认为该属性变成了方法。【注：没有 this 修饰的是函数的局部变量，注意两者的区别。】
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<script type="text/javascript">
    function Person(name, age) {
        this.name = name;
        this.age = age;
        this.info = function() {
            document.writeln("name: " + this.name + ", age: " + this.age);
        };
    }
    var p1 = new Person("Tom", 20);
    p1.info();

    document.writeln("<br>");

    var p2 = new Person("Ken", 23);
    p2.info();
</script>
</body>
</html>
```

输出结果：
```
name: Tom, age: 20 
name: Ken, age: 23
```


但使用上面方法为 `Person` 类定义增加 `info` 方法相当不好，主要有如下两个原因：
- 性能低下：因为每次创建 `Person` 实例时，程序依次向下执行，每次都将创建一个新的 `info` 函数——当创建多个 `Person` 对象时，系统就会有很多个 `info` 函数——这就会造成系统内存泄漏，从而引起性能下降。
- 使得 `info` 函数中的局部变量产生闭包：闭包会扩大局部变量的作用域，使得局部变量一直存活到函数之外的地方。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<script type="text/javascript">
    function Person() {
        var a = 10;
        this.info = function () {
            document.writeln(a);
            return a;
        };
    }

    var p = new Person();
    var a = p.info();

    document.writeln("<br>");
    document.writeln(a);
</script>
</body>
</html>
```

输出结果：
```
10 
10
```

为了避免这两种情况，通常不建议直接在函数定义（也就是类定义）中直接为该函数定义方法，而是建议使用 `prototype` 属性。

JavaScript 的所有类（也就是函数）都有一个 `prototype` 属性，当我们为 JavaScript 类的 `prototype` 属性增加函数、属性时，则可视为对原有类的扩展。我们可理解为：增加了 `prototype` 属性的类继承了原有的类——这就是 `JavaScript` 所提供的**伪继承**机制。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<script type="text/javascript">
    function Person(name, age) {
        this.name = name;
        this.age = age;
        this.info = function () {
            document.writeln("name: " + this.name + ", age: " + this.age + "<br>");
        };
    }

    var p1 = new Person("Tom", 29);
    p1.info();

    Person.prototype.hello = function () {
        document.writeln("Hello, I am " + this.name + "!<br>");
    };

    var p2 = new Person("Ken", 30);
    p2.info();
    p2.hello();

    p1.hello();
</script>
</body>
</html>
```

输出结果：
```
name: Tom, age: 29
name: Ken, age: 30
Hello, I am Ken!
Hello, I am Tom!
```


**注意：JavaScript 并没有提供真正的继承，当通过某个类的 `prototype` 属性动态地增加属性或方法时，其实质是对原有类的修改，并不是真正产生一个新的子类，所以这种机制依然只是一种伪继承。**

通过使用 `prototype` 属性，可以对 JavaScript 的内置类进行扩展。
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<script type="text/javascript">
    Array.prototype.indexOf = function (obj) {
        var result = -1;
        for (var i = 0; i < this.length; i++) {
            if (this[i] == obj) {
                result = i;
                break;
            }
        }
        return result;
    };
    var array = [2, 45, 87, 90];
    document.writeln(array.indexOf(45) + "<br>");
    document.writeln(array.indexOf(86));
</script>
</body>
</html>
```

输出结果：
```
1
-1
```

**再次提醒：尽量避免使用内嵌函数为类定义方法，而应该使用增加 `prototype` 属性的方式来增加方法。通过 `prototype` 属性来为类动态地增加属性和方法会让程序更加安全，性能更加稳定。**

## 13.11 创建对象

JavaScript 中创建对象大致有三种方式：
- 使用 `new` 关键字调用构造器创建对象。【注：构造器就是函数】
- 使用 `Object` 直接创建对象。
- 使用 JSON 语法创建对象。

### 13.11.1 使用 `new` 关键字调用构造器创建对象

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<script type="text/javascript">
    function Person(name, age) {
        this.name = name;
        this.age = age;
    }
    var p = new Person("Ken", 23);
    document.writeln("name: " + p.name + ", age: " + p.age);
</script>
</body>
</html>
```

输出结果：
```
name: Ken, age: 23
```

### 13.11.2 使用 `Object` 直接创建对象

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<script type="text/javascript">
    var p = new Object();
    p.name = "Tom";
    p.age = 34;
    document.writeln("name: " + p.name + ", age: " + p.age);
</script>
</body>
</html>
```

输出结果：
```
name: Tom, age: 34
```

将一个已有的函数添加为对象的方法
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<script type="text/javascript">
    var p = new Object();
    p.name = "Tom";
    p.age = 36;

    function foo() {
        document.writeln("name: " + p.name + ", age: " + p.age);
    }

    p.info = foo;
    p.info();
</script>
</body>
</html>
```

输出结果：
```
name: Tom, age: 36
```

通过 `new Function()` 定义匿名函数为对象增加方法
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<script type="text/javascript">
    var p = new Object();
    p.name = "Tom";
    p.age = 35;
    p.info = new Function('document.writeln("name: " + p.name + ", age: " + p.age);');
    p.info();
</script>
</body>
</html>
```

输出结果：
```
name: Tom, age: 35
```


### 13.11.3 使用 JSON 语法创建对象

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<script type="text/javascript">
    var ken =
        {
            name: "Tom",
            age: 36,
            hobby: ["Basketball", "Swimming"],
            parents: [
                {
                    name: "Ken",
                    age: 67
                },
                {
                    name: "Marry",
                    age: 60
                }
            ]
        };
    document.writeln(ken.parents);
</script>
</body>
</html>
```

输出结果：
```
[object Object],[object Object]
```