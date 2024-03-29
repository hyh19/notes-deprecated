[TOC]

# 第3章 Kotlin语言基础

https://github.com/EasyKotlin/chapter3_kotlin_basics

## 3.1 包（package）

跳过

## 3.2 声明变量和值

> 在Kotlin中， 一切都是对象。所以，所有变量也都是对象（也就是说，任何变量都是根据引用类型来使用的）。
> 
> Kotlin的变量分为 `var` (可变的) 和 `val` (不可变的)。
> 
> 可以简单理解为：
> - `var` 是可写的，在它生命周期中可以被多次赋值； 
> - 而 `val` 是只读的，仅能一次赋值，后面就不能被重新赋值。 

```
fun main(args: Array<String>) {
    var a = 1
    a = 2
    println(a)
    println(a::class)
    println(a::class.java)

    var x = 5 // 自动推断出 `Int` 类型
    x += 1
    println("x = $x")
}
```

输出结果：
```
2
class kotlin.Int
int
x = 6
```

```
fun main(args: Array<String>) {
    val b = "a"
    // b = "b" // 编译器会报错： Val cannot be reassigned
    println(b)
    println(b::class)
    println(b::class.java)
}
```

输出结果：
```
a
class kotlin.String
class java.lang.String
```

```
fun main(args: Array<String>) {
    val c: Int = 1  // 立即赋值（注：推荐这种写法，明确指定类型并立即赋值）
    val d = 2   // 自动推断出 `Int` 类型
    // val e // 如果没有初始赋值，类型不能省略。
    val e: Int
    e = 3       // 明确赋值
    println("c = $c, d = $d, e = $e")
}
```

输出结果：
```
c = 1, d = 2, e = 3
```

> 在Kotlin中，更简洁的、更常用的场景是：只要可能，尽量在Kotlin中首选使用`val`不变值。因为事实上在程序中大部分地方使用不可变的变量，可带来很多益处，如：可预测的行为和线程安全。

## 3.3 变量类型推断

### 3.3.1 省去变量类型

> 在Kotlin中**大部分情况**你不需要说明你使用对象的类型，编译器可以直接推断出它的类型。

```
import java.util.*

fun main(args: Array<String>) {
    val str = "abc"
    println(str)
    println(str is String)
    println(str::class)
    println(str::class.java)
    println("========")
    val d = Date()
    println(d)
    println(d is Date)
    println(d::class)
    println(d::class.java)
    println("========")
    val bool = true
    println(bool)
    println(bool::class)
    println(bool::class.java)
    println("========")
    val array = arrayOf(1, 2, 3)
    println(array)
    println(array is Array)
    println(array::class)
    println(array::class.java)
}
```

输出结果：
```
abc
true
class kotlin.String
class java.lang.String
========
Sun Mar 18 12:53:55 CST 2018
true
class java.util.Date
class java.util.Date
========
true
class kotlin.Boolean
boolean
========
[Ljava.lang.Integer;@759ebb3d
true
class kotlin.Array
class [Ljava.lang.Integer;
```


> 但是，类型推断不是万能的。例如，整型变量Int不能赋值Long变量。
```
fun main(args: Array<String>) {
    val x: Int = 10
    // val y: Long = x // 编译错误：Type mismatch
    val y: Long = x.toLong() // 要先做类型转换
}
```

### 3.3.2 使用is运算符进行类型检测

> `is` 运算符检测一个表达式是否某类型的一个实例。
> 
> 如果一个不可变的局部变量或属性已经判断出为某类型，那么检测后的分支中可以直接当作该类型使用，无需显式转换。
```
fun printLength(obj: Any) {
    if (obj is String) {
        // `obj` 在该条件分支内自动转换成 `String`
        println(obj::class)
        println(obj.length)
    } else {
        // 在离开类型检测分支后，`obj` 仍然是 `Any` 类型。
        println(obj::class)
    }
}

fun main(args: Array<String>) {
    val obj1 = "Hello"
    printLength(obj1)

    val obj2 = Any()
    printLength(obj2)
}
```

输出结果：
```
class kotlin.String
5
class kotlin.Any
```

学习到这

## 3.4 字符串与其模板表达式

> 原始字符串(raw string)由三重引号（"""）分隔(这个跟python一样)。原始字符串可以包含换行符和任何其他字符。
> 
> 字符串可以包含模板表达式。模板表达式以美元符号（$）开始。
```
fun main(args: Array<String>) {
    val rawString = """
fun helloWorld(val name : String) {
   println("Hello, world!")
}
"""
    println(rawString)

    val fooTemplateString = "$rawString has ${rawString.length} characters"
    println(fooTemplateString)
}
```

## 3.5 流程控制语句

### 3.5.1 if表达式

> 在 Kotlin 中，if 是一个表达式，即它会返回一个值(跟Scala一样)。
```
fun max(a: Int, b: Int): Int {
    // 作为表达式
    val max = if (a > b) a else b
    return max
    // return if (a > b) a else b // 这样写也可以
}

fun main(args: Array<String>) {
    println(max(10, 20))
}
```

> 另外，if 的分支可以是代码块，最后的表达式作为该块的值。
```
fun max(a: Int, b: Int): Int {
    val max = if (a > b) {
        println("Max is a")
        a
    } else {
        println("Max is b")
        b
    }
    return max
}

fun main(args: Array<String>) {
    println(max(10, 20))
}
```

传统用法
```
fun max(a: Int, b: Int): Int {
    var max = a
    // 传统用法
    if (a < b) max = b
    return max
}

fun main(args: Array<String>) {
    println(max(10, 20))
}
```

if-else
```
fun max(a: Int, b: Int): Int {
    var max: Int
    if (a > b) {
        max = a
    } else {
        max = b
    }
    return max
}

fun main(args: Array<String>) {
    println(max(10, 20))
}
```

> ~~如果 if 表达式只有一个分支， 或者分支的结果是 Unit , 它的值就是 Unit 。~~【注：新版本的编译器不再如此】
```
>>> val x = if (1 == 1) true
error: 'if' must have both main and 'else' branches if used as an expression
val x = if (1 == 1) true
        ^

>>> val y = if (1 == 1) true else false
>>> y
true
```

> if后的括号不能省略，括号里表达式的值须是布尔型。
```
>>> if ("a") 1
error: type mismatch: inferred type is String but Boolean was expected
if ("a") 1
    ^

>>> if (1) println(1)
error: the integer literal does not conform to the expected type Boolean
if (1) println(1)
    ^
```

### 3.5.2 when表达式

> when表达式类似于 switch-case 表达式。when会对所有的分支进行检查直到有一个条件满足。如果其他分支都不满足条件会到 else 分支（类似default）。
```
fun cases(obj: Any) {
    when (obj) {
        1 -> println("第一项")
        "hello" -> println("这个是字符串hello")
        is Long -> println("这是一个Long类型数据")
        !is String -> println("这不是String类型的数据")
        else -> println("else类似于Java中的default")
    }
}

fun main(args: Array<String>) {
    cases(1)
    cases("hello")
    cases(10L)
    cases(20)
    cases("John")
}
```

> 如果我们有很多分支需要用相同的方式处理，则可以把多个分支条件放在一起，用逗号分隔。
```
fun switch(x: Any) {
    when (x) {
        -1, 0 -> println("x == -1 or x == 0")
        1 -> println("x == 1")
        2 -> println("x == 2")
        else -> { // 注意这个块
            println("x is neither 1 nor 2")
        }
    }
}

fun main(args: Array<String>) {
    switch(-1)
    switch(0)
    switch(2)
    switch(20)
}
```

> 我们可以用任意表达式（而不只是常量）作为分支条件。
```
import java.lang.Integer.parseInt

fun switch(x: Int) {
    val s = "123"
    when (x) {
        -1, 0 -> println("x == -1 or x == 0")
        1 -> println("x == 1")
        2 -> println("x == 2")
        8 -> println("x is 8")
        parseInt(s) -> println("x is 123")
        else -> { // 注意这个块
            println("x is neither 1 nor 2")
        }
    }
}

fun main(args: Array<String>) {
    switch(-1)
    switch(0)
    switch(1)
    switch(123)
    switch(20)
}
```

> 我们也可以检测一个值在 in 或者不在 !in 一个区间或者集合中。
```
fun switch(x: Int) {
    val validNumbers = arrayOf(1, 2, 3)
    when (x) {
        in 1..10 -> println("x is in the range")
        in validNumbers -> println("x is valid")
        !in 10..20 -> println("x is outside the range")
        else -> println("none of the above")
    }
}

fun main(args: Array<String>) {
    switch(5)
    switch(2)
    switch(25)
    switch(15)
}
```

### 3.5.3 for循环

> for 循环可以对任何提供迭代器（iterator）的对象进行遍历。

```
fun main(args: Array<String>) {
    for (arg in args) {
        println(arg)
    }

    for (i in args.indices) {
        println(args[i])
    }

    for ((index, value) in args.withIndex()) {
        println("the element at $index is $value")
    }
}
```

https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/with-index.html

### 3.5.4 while循环

```
fun main(args: Array<String>) {
    var x = 10
    while (x > 0) {
        println(x)
        x--
    }

    var y = 10
    do {
        y++
        println(y)
    } while (y < 20)
}
```

### 3.5.5 break 和 continue

```
fun main(args: Array<String>) {
    for (i in 1..10) {
        if (i % 2 == 0) {
            break
        }
        println(i)
    }
}
```

```
fun main(args: Array<String>) {
    for (i in 1..10) {
        if (i % 2 == 0) {
            continue
        }
        println(i)
    }
}
```

### 3.5.6 return返回

> 在Kotlin中，除了表达式的值，有返回值的函数都要求显式使用`return`来返回其值。
```
fun sum(a: Int, b: Int): Int {
    return a + b
}

fun max(a: Int, b: Int): Int {
    if (a > b) {
        return a
    } else {
        return b
    }
}

fun main(args: Array<String>) {
    println(sum(10, 20))
    println(max(10, 20))
}
```

> 我们在Kotlin中，可以直接使用`=`符号来直接返回一个函数的值。
```
fun sum(a: Int, b: Int) = a + b

fun max(a: Int, b: Int) = if (a > b) a else b

fun main(args: Array<String>) {
    println(sum(10, 20))
    println(max(10, 20))
}
```

没加括号：返回值是一个整型\
加了括号：返回值是一个函数
```
>>> val sum = fun(a: Int, b: Int) = a + b
>>> sum
(kotlin.Int, kotlin.Int) -> kotlin.Int
>>> sum(10, 20)
30
>>> val sumf = fun(a: Int, b: Int) = { a + b }
>>> sumf
(kotlin.Int, kotlin.Int) -> () -> kotlin.Int
>>> sumf(10, 20).invoke()
30
>>> val max = fun(a: Int, b: Int) = if (a > b) a else b
>>> max
(kotlin.Int, kotlin.Int) -> kotlin.Int
>>> max(10, 20)
20
>>> val maxf = fun(a: Int, b: Int) = { if (a > b) a else b }
>>> maxf
(kotlin.Int, kotlin.Int) -> () -> kotlin.Int
>>> maxf(10, 20).invoke()
20
```

> kotlin 中 `return` 语句会从最近的函数或匿名函数中返回，但是在Lambda表达式中遇到return，则直接返回最近的外层函数。


```
fun returnDemo_1() {
    println("START: " + ::returnDemo_1.name)
    val intArray = intArrayOf(1, 2, 3, 4, 5)
    intArray.forEach {
        if (it == 3) return
        println(it)
    }
    println("END: " + ::returnDemo_1.name)
}

fun main(args: Array<String>) {
    returnDemo_1()
}
```

输出结果：
```
START: returnDemo_1
1
2
```


```
fun returnDemo_2() {
    println("START: " + ::returnDemo_2.name)
    val intArray = intArrayOf(1, 2, 3, 4, 5)
    intArray.forEach(fun(a: Int) {
        if (a == 3) return
        println(a)
    })
    println("END: " + ::returnDemo_2.name)
}

fun main(args: Array<String>) {
    returnDemo_2()
}
```

输出结果：
```
START: returnDemo_2
1
2
4
5
END: returnDemo_2
```

> 在`returnDemo_2` 中，我们用一个匿名函数替代 lambda 表达式。 匿名函数内部的 return 语句将从该匿名函数自身返回。

https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/for-each.html

### 3.5.7 标签（label）

> 在 Kotlin 中任何表达式都可以用标签（label）来标记。 标签的格式为标识符后跟 `@` 符号，例如：`abc@`、`jarOfLove@` 都是有效的标签。我们可以用Label标签来控制 `return`、`break` 或 `continue`的跳转（jump）行为。

```
fun returnDemo_3() {
    println("START: " + ::returnDemo_3.name)
    val intArray = intArrayOf(1, 2, 3, 4, 5)
    intArray.forEach here@{
        if (it == 3) return@here
        println(it)
    }
    println("END: " + ::returnDemo_3.name)
}

fun main(args: Array<String>) {
    returnDemo_3()
}
```

输出结果：
```
START: returnDemo_3
1
2
4
5
END: returnDemo_3
```

> 另外，我们也可以使用隐式标签更方便。 该标签与接收该 lambda 的函数同名。
```
fun returnDemo_4() {
    println("START: " + ::returnDemo_4.name)
    val intArray = intArrayOf(1, 2, 3, 4, 5)
    intArray.forEach {
        if (it == 3) return@forEach // 从 lambda 表达式 @forEach 中返回。
        println(it)
    }

    println("END: " + ::returnDemo_4.name)
}

fun main(args: Array<String>) {
    returnDemo_4()
}
```

输出结果：
```
START: returnDemo_4
1
2
4
5
END: returnDemo_4
```

> 当我们想直接跳转到外层for循环，这个时候我们就可以使用标签了。
```
fun breakDemo_2() {
    println("---------------  breakDemo_2 ---------------")
    outer@ for (outer in 1..5) {
        println("outer ==== " + outer)
        for (inner in 1..10) {
            println("inner = " + inner)
            if (inner % 2 == 0) {
                break@outer
            }
        }
    }
}

fun main(args: Array<String>) {
    breakDemo_2()
}
```

输出结果：
```
---------------  breakDemo_2 ---------------
outer ==== 1
inner = 1
inner = 2
```

> 有时候，为了代码可读性，我们可以用标签来显式地指出循环体的跳转地址。
```
fun breakDemo_3() {
    println("---------------  breakDemo_3 ---------------")
    for (outer in 1..5) {
        println("outer ==== " + outer)
        inner@ for (inner in 1..10) {
            println("inner = " + inner)
            if (inner % 2 == 0) {
                break@inner
                // break // 效果一样
            }
        }
    }
}

fun main(args: Array<String>) {
    breakDemo_3()
}
```

输出结果：
```
---------------  breakDemo_3 ---------------
outer ==== 1
inner = 1
inner = 2
outer ==== 2
inner = 1
inner = 2
outer ==== 3
inner = 1
inner = 2
outer ==== 4
inner = 1
inner = 2
outer ==== 5
inner = 1
inner = 2
```

### 3.5.8 throw表达式

> 在 Kotlin 中 throw 是表达式，它的类型是特殊类型 Nothing。 该类型没有值。跟C、Java中的`void` 意思一样。
```
>>> Nothing::class
class java.lang.Void
```

> 我们在代码中，用 Nothing 来标记无返回的函数。
```
>>> fun fail(msg: String): Nothing { throw IllegalArgumentException(msg) }
>>> fail("Hello World")
java.lang.IllegalArgumentException: Hello World
	at Line_1.fail(Unknown Source)
```

> 如果把一个throw表达式的值赋值给一个变量，需要显式声明类型为`Nothing`。另外，因为Nothing类型的变量没有任何值，所以无法当做参数传给函数：
```
>>> val ex = throw Exception("Hello World")
error: 'Nothing' property type needs to be specified explicitly
val ex = throw Exception("Hello World")
    ^

>>> val ex: Nothing = throw Exception("Hello World")
java.lang.Exception: Hello World
>>> println(ex)
error: unresolved reference: ex
println(ex)
        ^

>>> ex
error: unresolved reference: ex
ex
^
```

## 3.6 代码注释

N/A

## 3.7 语法与标识符

### 3.7.1 修饰符

N/A

### 3.7.2 关键字(保留字)

#### `this` 关键字

> 在类的成员中，this 指向的是该类的当前对象。
```
class ThisDemo {
    val thisis = "THIS IS"

    fun howIsThis() {
        println("HOW IS THIS?")
    }

    fun whatIsThis(): ThisDemo {
        println(this.thisis)
        this.howIsThis();
        return this
    }
}

fun main(args: Array<String>) {
    val demo = ThisDemo()
    println(demo.whatIsThis())
}
```

> 在扩展函数或者带接收者的函数字面值中， this 表示在点左侧传递的 接收者参数。
```
>>> val sum = fun Int.(x: Int): Int = this + x
>>> sum
kotlin.Int.(kotlin.Int) -> kotlin.Int
>>> 1.sum(1)
2
>>> val concat = fun String.(x: Any) = this + x
>>> concat
kotlin.String.(kotlin.Any) -> kotlin.String
>>> "abc".concat(123)
abc123
>>> "abc".concat(true)
abctrue
```

> 如果 this 没有限定符，它指的是最内层的包含它的作用域。如果我们想要引用其他作用域中的 this，可以使用 this@label 标签。
```
class Outer {
    val oh = "Oh!"

    inner class Inner {

        fun m() {
            val outer = this@Outer
            val inner = this@Inner
            val pthis = this
            println("outer: " + outer)
            println("inner: " + inner)
            println("pthis: " + pthis)
            println(this@Outer.oh)

            val fun1 = hello@ fun String.() {
                val d1 = this // fun1 的接收者
                println("d1: " + d1)
            }
            "abc".fun1()

            val fun2 = { s: String ->
                val d2 = this
                println("d2: " + d2)
            }
            fun2("abc")
        }
    }
}

fun main(args: Array<String>) {
    val outer = Outer()
    outer.Inner().m()
}
```

输出结果：
```
outer: Outer@4d7e1886
inner: Outer$Inner@3cd1a2f1
pthis: Outer$Inner@3cd1a2f1
Oh!
d1: abc
d2: Outer$Inner@3cd1a2f1
```

#### super 关键字

> super关键字持有指向其父类的引用。

```
open class Father {
    open val firstName = "Chen"
    open val lastName = "Jason"
    fun ff() {
        println("FFF")
    }
}

class Son : Father {
    override var firstName = super.firstName
    override var lastName = "Jack"

    constructor(lastName: String) {
        this.lastName = lastName
    }

    fun love() {
        super.ff()
        println(super.firstName + " " + super.lastName + " Love " + this.firstName + " " + this.lastName)
    }

}

fun main(args: Array<String>) {
    val son = Son("Harry")
    son.love()
}
```

输出结果：
```
FFF
Chen Jason Love Chen Harry
```

### 3.7.3 操作符和操作符的重载

N/A

### 3.7.4 操作符优先级（Precedence)

N/A

### 3.7.5 一元操作符（unary operation）

#### 前缀操作符

| 表达式     | 翻译为        |
|------------|---------------|
| `+a` | `a.unaryPlus()` |
| `-a` | `a.unaryMinus()` |
| `!a` | `a.not()` |

> 以下是如何重载一元减运算符的示例：
```
data class Point(val x: Int, val y: Int)

operator fun Point.unaryMinus() = Point(-x, -y)

fun main(args: Array<String>) {
    val p = Point(1, 1)
    val np = -p
    println(np)
}
```

输出结果：
```
Point(x=-1, y=-1)
```

####  递增和递减

| 表达式     | 翻译为        |
|------------|---------------|
| `a++` | `a.inc()` 返回值是`a`  |
| `a--` | `a.dec()`  返回值是`a`|
| `++a` | `a.inc()`  返回值是`a+1`|
| `--a` | `a.dec()`  返回值是`a-1` |

### 3.7.6 二元操作符

#### 算术运算符

| 表达式     | 翻译为        |
| -----------|-------------- |
| `a + b` | `a.plus(b)` |
| `a - b` | `a.minus(b)` |
| `a * b` | `a.times(b)` |
| `a / b` | `a.div(b)` |
| `a % b` | `a.rem(b)`、 `a.mod(b)`  |
| `a..b ` | `a.rangeTo(b)` |

#### 字符串的`+`运算符重载

> 在Kotlin中1+""是不允许的
```
>>> "" + 1
1
>>> 1 + ""
error: none of the following functions can be called with the arguments supplied: 
public final operator fun plus(other: Byte): Int defined in kotlin.Int
public final operator fun plus(other: Double): Double defined in kotlin.Int
public final operator fun plus(other: Float): Float defined in kotlin.Int
public final operator fun plus(other: Int): Int defined in kotlin.Int
public final operator fun plus(other: Long): Long defined in kotlin.Int
public final operator fun plus(other: Short): Int defined in kotlin.Int
1 + ""
  ^

>>> 
```

#### 自定义重载的 `+` 运算符

```
data class Counter(var index: Int)

operator fun Counter.plus(increment: Int): Counter {
    return Counter(index + increment)
}

fun main(args: Array<String>) {
    val c = Counter(1)
    val cplus = c + 10
    println(cplus)
}
```

输出结果：
```
Counter(index=11)
```

####  `in`操作符

| 表达式     | 翻译为        |
| -----------|-------------- |
| `a in b` | `b.contains(a)` |
| `a !in b` | `!b.contains(a)` |

#### 索引访问操作符

| 表达式 | 翻译为        |
| -------|-------------- |
| `a[i]`  | `a.get(i)` |
| `a[i] = b` | `a.set(i, b)` |

#### 调用操作符

| 表达式 | 翻译为        |
|--------|---------------|
| `a()`  | `a.invoke()` |
| `a(i)`  | `a.invoke(i)` |

#### 计算并赋值

| 表达式     | 翻译为        |
|------------|---------------|
| `a += b` | `a.plusAssign(b)` |
| `a -= b` | `a.minusAssign(b)` |
| `a *= b` | `a.timesAssign(b)` |
| `a /= b` | `a.divAssign(b)` |
| `a %= b` | `a.modAssign(b)` |

> 对于赋值操作，例如 `a += b`，编译器会试着生成 `a = a + b` 的代码（这里包含类型检查：`a + b` 的类型必须是 `a` 的子类型）。

#### 相等与不等操作符

> Kotlin 中有两种类型的相等性：
> 
> * 引用相等 `===` `!==`（两个引用指向同一对象）
> * 结构相等 `==` `!=`（ 使用`equals()` 判断）

| 表达式     | 翻译为        |
|------------|---------------|
| `a == b` | `a?.equals(b) ?: (b === null)` |
| `a != b` | `!(a?.equals(b) ?: (b === null))` |

> 这个 `==` 操作符有些特殊：它被翻译成一个复杂的表达式，用于筛选 `null` 值。
> 
> 意思是：如果 a 不是 null 则调用 `equals(Any?)` 函数并返回其值；否则（即 `a === null`）就计算 `b === null` 的值并返回。
> 
> 当与 null 显式比较时，`a == null` 会被自动转换为 `a=== null`
> 
> *注意*：`===` 和 `!==`不可重载。

#### Elvis 操作符 `?:`

> 在Kotin中，Elvis操作符特定是跟null比较。也就是说
> ```
> y = x?:0
> ```
> 等价于
> ```
> val y = if(x!==null) x else 0
> ```
> 
> 主要用来作`null`安全性检查。

```
>>> val x = null
>>> val y = x?:0
>>> y
0
>>> val x = false
>>> val y = x?:0
>>> y
false
>>> val x = ""
>>> val y = x?:0
>>> y

>>> val x = "abc"
>>> val y = x?:0
>>> y
abc
```

#### 比较操作符

| 表达式 | 翻译为        |
|--------|---------------|
| `a > b`  | `a.compareTo(b) > 0` |
| `a < b`  | `a.compareTo(b) < 0` |
| `a >= b` | `a.compareTo(b) >= 0` |
| `a <= b` | `a.compareTo(b) <= 0` |

> 所有的比较都转换为对 `compareTo` 的调用，这个函数需要返回 `Int` 值。

#### 用infix函数自定义中缀操作符

我们可以通过自定义infix函数来实现中缀操作符。

```
data class Person(val name: String, val age: Int)

infix fun Person.grow(years: Int): Person {
    return Person(name, age + years)
}

fun main(args: Array<String>) {
    val person = Person("Jack", 20)

    println(person.grow(2))

    println(person grow 2) // 如果没有修饰符 infix 会报错
}
```

输出结果：
```
Person(name=Jack, age=22)
Person(name=Jack, age=22)
```

## 3.8 函数扩展和属性扩展(Extensions)

### 3.8.1 扩展函数

```
fun String.notEmpty(): Boolean {
    return !this.isEmpty()
}

fun main(args: Array<String>) {
    val a = "Hello World"
    println(a.notEmpty())

    val b = ""
    println(b.notEmpty())
}
```

输出结果：
```
true
false
```

```
fun MutableList<Int>.swap(index1: Int, index2: Int) {
    val tmp = this[index1]
    this[index1] = this[index2]
    this[index2] = tmp
}

fun main(args: Array<String>) {
    val mList = mutableListOf<Int>(10, 20, 30, 40, 50)
    println(mList)
    mList.swap(0, mList.size - 1)
    println(mList)
}
```

输出结果：
```
[10, 20, 30, 40, 50]
[50, 20, 30, 40, 10]
```

> 为了在接收者类型表达式中使用泛型，我们要在函数名前声明泛型参数。
```
fun <T> MutableList<T>.swap(index1: Int, index2: Int) {
    val tmp = this[index1]
    this[index1] = this[index2]
    this[index2] = tmp
}

fun main(args: Array<String>) {
    val mList = mutableListOf<Int>(10, 20, 30, 40, 50)
    println(mList)
    mList.swap(0, mList.size - 1)
    println(mList)

    val mmList = mutableListOf<String>("one", "two", "three", "four", "five")
    println(mmList)
    mmList.swap(0, mmList.size - 1)
    println(mmList)
}
```

输出结果：
```
[10, 20, 30, 40, 50]
[50, 20, 30, 40, 10]
[one, two, three, four, five]
[five, two, three, four, one]
```

> 扩展不是真正的修改他们所扩展的类。我们定义一个扩展，其实并没有在一个类中插入新函数，仅仅是通过该类型的变量，用点`.`表达式去调用这个新函数。

### 3.8.2 扩展属性

```
val <T> List<T>.lastIndex: Int get() = size - 1

fun main(args: Array<String>) {
    val mList = mutableListOf<Int>(10, 20, 30, 40, 50)
    println(mList.lastIndex)
}
```

输出结果：
```
4
```

> 注意：由于扩展没有实际的将成员插入类中，因此对扩展的属性来说，它的行为只能由显式提供的 getters/setters 定义。

## 3.9 空指针安全(Null-safety)

> 在Kotlin中`null`等同于空指针。我们通过代码来看一下`null`的有趣的特性：
> 
> 一个非空引用不能直接赋值为`null` :
```
>>> var a = "Hello World"
>>> a = null
error: null can not be a value of a non-null type String
a = null
    ^

>>> var one = 1
>>> one = null
error: null can not be a value of a non-null type Int
one = null
      ^

>>> var arrayInts = intArrayOf(1, 2, 3)
>>> arrayInts = null
error: null can not be a value of a non-null type IntArray
arrayInts = null
            ^
```

> 如果要允许为空，我们可以在变量的类型后面加个问号`?`声明一个变量为可空的：

```
>>> var a: String? = "Hello World"
>>> a = null
>>> var one: Int? = 1
>>> one = null
>>> var arrayInts: IntArray? = intArrayOf(1, 2, 3)
>>> arrayInts = null
>>> a
null
>>> one
null
>>> arrayInts
null
```

> 如果我们声明了一个可空`String?`类型变量`na` ，然后直接调用`length`属性，这将是不安全的，编译器会直接报错。我们使用安全调用`?.` 和 非空断言调用 `!!.`
```
>>> var na: String? = "Hello World"
>>> na.length
error: only safe (?.) or non-null asserted (!!.) calls are allowed on a nullable receiver of type String?
na.length
  ^

>>> na?.length
11
>>> na!!.length
11
>>> na = null
>>> na.length
error: only safe (?.) or non-null asserted (!!.) calls are allowed on a nullable receiver of type String?
na.length
  ^

>>> na?.length
null
>>> na!!.length
kotlin.KotlinNullPointerException
```

> 如果要只对非空值执行某个操作，安全调用操作符可以与 `let` （以调用者的值作为参数来执行指定的函数块，并返回其结果）一起使用：
```
fun main(args: Array<String>) {
    val listWithNulls: List<String?> = listOf("A", "B", null)
    println(listWithNulls)

    listWithNulls.forEach {
        it?.let { println(it) }
    }
}
```

输出结果：
```
[A, B, null]
A
B
```

学习结束 2018.3.18