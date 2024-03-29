[TOC]

# 第6章 函数和Lambda表达式

## 6.1 函数入门

### 6.1.1 定义和调用函数

```
package ch611

/**
 * 6.1.1 定义和调用函数
 */

fun max(x: Int, y: Int): Int {
    val z = if (x > y) x else y
    return z
}

// 返回一个表达式
fun max1(x: Int, y: Int): Int {
    return if (x > y) x else y
}

fun sayHi(name: String): String {
    return "Hello, ${name}!"
}

fun main(args: Array<String>) {
    var a = 6
    var b = 9
    var result = max(a, b)
    println("max: ${result}")

    result = max1(a, b)
    println("max1: ${result}")

    println(sayHi("John"))
}
```

### 6.1.2 函数返回值和Unit

```
package ch612

/**
 * 6.1.2 函数返回值和Unit
 */

fun foo() {
    println("Hello, world!")
}

fun sayHi(name: String): Unit {
    println("Hello, $name!")
}

fun showMsg(msg: String, count: Int) {
    for (i in 1..count) {
        println(msg)
    }
}

fun main(args: Array<String>) {
    foo()
    sayHi("Ken")
    showMsg("Kotlin", 3)
}
```

### 6.1.3 递归函数

跳过

### 6.1.4 单表达式函数

```
package ch614

/**
 * 6.1.4 单表达式函数
 */

fun area(x: Double, y: Double): Double = x * y

fun area1(x: Double, y: Double) = x * y // 省略返回值类型

fun main(args: Array<String>) {
    println(area(5.0, 10.0))
    println(area1(4.0, 8.0))
}
```

## 6.2 函数的形参

### 6.2.1 命名参数

```
package ch621

/**
 * 6.2.1 命名参数
 */

fun girth(width: Double, height: Double) = 2 * (width + height)

fun main(args: Array<String>) {
    println(girth(3.5, 4.8))
    println(girth(width = 3.5, height = 4.8))
    println(girth(height = 4.8, width = 3.5))
    println(girth(3.5, height = 4.8))
    // 位置参数必须放在命名参数之前
    // println(girth(width = 3.5, 4.8)) // Mixing named and positioned arguments is not allowed
}
```

### 6.2.2 形参默认值

```
package ch622

/**
 * 6.2.2 形参默认值
 */

fun sayHi(name: String = "John", message: String = "Welcome!") {
    println("Hi, $name!")
    println("Message: $message")
}

fun main(args: Array<String>) {
    sayHi()
    sayHi("Ken")
    sayHi("Mike", "Welcome to Beijing!")
    sayHi(message = "Welcome to Guangzhou!")

    // sayHi("Welcome to Shenzhen!", name = "Jim") // An argument is already passed for this parameter
}
```

```
package ch622

/**
 * 6.2.2 形参默认值
 */

// 定义一个打印三角形的函数
fun printTriangle(height: Int = 5, char: Char) {
    for (i in 1..height) {
        // 先打印一排空格
        for (j in 0 until height - i) {
            print(" ");
        }
        // 再打印一排特殊字符
        for (j in 0 until 2 * i - 1) {
            print(char);
        }
        println()
    }
}

fun main(args: Array<String>) {
    printTriangle(char = '*')
    printTriangle(6, '@')
    printTriangle(7, char = '#')

    // printTriangle('#') // The character literal does not conform to the expected type Int
}
```

### 6.2.3 尾递归函数

跳过

### 6.2.4 个数可变的形参

```
package ch624

/**
 * 6.2.4 个数可变的形参
 */

fun test(a: Int, vararg books: String) {
    for (b in books) {
        println(b)
    }
    println(a)
}

fun main(args: Array<String>) {
    test(5, "疯狂iOS讲义", "疯狂Android讲义")
}
```

```
package ch624

/**
 * 6.2.4 个数可变的形参
 */

fun test(num: Int, vararg books: String) {
    println(num)
    for (b in books) {
        println(b)
    }
}

fun test1(vararg books: String, num: Int) {
    println(num)
    for (b in books) {
        println(b)
    }
}

fun main(args: Array<String>) {
    test(5, "疯狂iOS讲义", "疯狂Android讲义")

    test1("疯狂iOS讲义", "疯狂Android讲义", num = 20)

    var arr = arrayOf("疯狂Kotlin讲义", "疯狂Java讲义")
    test1(*arr, num = 20)
}
```

## 6.3 函数重载

```
package ch630

/**
 * 6.3 函数重载
 */

fun test() {
    println("test 1")
}

fun test(msg: String) {
    println(msg)
}

fun test(num: Int): String {
    println(num)
    return "test 3"
}

fun main(args: Array<String>) {
    test()
    test("Hello World")
    var result = test(30)
    println(result)
}
```

## 6.4 局部函数

```
package ch640

/**
 * 6.4 局部函数
 */

fun getMathFunc(type: String, num: Int): Int {

    fun square(n: Int): Int {
        return n * n
    }

    fun cube(n: Int): Int {
        return n * n * n
    }

    fun factorial(n: Int): Int {
        var result = 1
        for (index in 2..n) {
            result *= index
        }
        return result
    }

    when (type) {
        "square" -> return square(num)
        "cube" -> return cube(num)
        else -> return factorial(num)
    }
}

fun main(args: Array<String>) {
    println(getMathFunc("square", 3))
    println(getMathFunc("cube", 3))
    println(getMathFunc("", 3))
}
```

## 6.5 高阶函数

### 6.5.1 使用函数类型

```
package ch651

/**
 * 6.5.1 使用函数类型
 */

fun main(args: Array<String>) {

    var myFun: (Int, Int) -> Int

    fun pow(base: Int, exponent: Int): Int {
        var result = 1
        for (i in 1..exponent) {
            result *= base
        }
        return result
    }

    myFun = ::pow
    println(myFun(3, 4)) // 81

    fun area(width: Int, height: Int): Int {
        return width * height
    }

    myFun = ::area
    println(myFun(3, 4)) // 12
}
```

### 6.5.2 使用函数类型作为形参类型

```
package ch652

/**
 * 6.5.2 使用函数类型作为形参类型
 */

fun map(data: Array<Int>, fn: (Int) -> Int): Array<Int> {
    var result = Array<Int>(data.size, { 0 })
    for (i in data.indices) {
        result[i] = fn(data[i])
    }
    return result
}

fun square(n: Int): Int {
    return n * n
}

fun cube(n: Int): Int {
    return n * n * n
}

fun factorial(n: Int): Int {
    var result = 1
    for (index in 2..n) {
        result *= index
    }
    return result
}

fun main(args: Array<String>) {
    var data = arrayOf(1, 2, 3, 4, 5)
    println("original: ${data.contentToString()}")
    println("square: ${map(data, ::square).contentToString()}")
    println("cube: ${map(data, ::cube).contentToString()}")
    println("factorial: ${map(data, ::factorial).contentToString()}")
}
```

### 6.5.3 使用函数类型作为返回值类型

```
package ch653

/**
 * 6.5.3 使用函数类型作为返回值类型
 */

fun getMathFunc(type: String): (Int) -> Int {

    fun square(n: Int): Int {
        return n * n
    }

    fun cube(n: Int): Int {
        return n * n * n
    }

    fun factorial(n: Int): Int {
        var result = 1
        for (index in 2..n) {
            result *= index
        }
        return result
    }

    when (type) {
        "square" -> return ::square
        "cube" -> return ::cube
        else -> return ::factorial
    }
}

fun main(args: Array<String>) {
    var mathFunc = getMathFunc("square")
    println(mathFunc(5))
    mathFunc = getMathFunc("cube")
    println(mathFunc(5))
    mathFunc = getMathFunc("other")
    println(mathFunc(5))
}
```

## 6.6 局部函数与 Lambda 表达式

### 6.6.2 使用 Lambda 表达式代替局部函数

```
package ch662

/**
 * 6.6.2 使用 Lambda 表达式代替局部函数
 */

fun getMathFunc(type: String): (Int) -> Int {

    when (type) {
        "square" -> return { n: Int ->
            n * n
        }
        "cube" -> return { n: Int ->
            n * n * n
        }
        else -> return { n: Int ->
            var result = 1
            for (index in 2..n) {
                result *= index
            }
            result
        }
    }
}

fun main(args: Array<String>) {
    var mathFunc = getMathFunc("square")
    println(mathFunc(5))
    mathFunc = getMathFunc("cube")
    println(mathFunc(5))
    mathFunc = getMathFunc("other")
    println(mathFunc(5))
}
```

### 6.6.3 Lambda 表达式的脱离

```
package ch663

/**
 * 6.6.3 Lambda 表达式的脱离
 */

var lambdaList = java.util.ArrayList<(Int) -> Int>()

fun collectFn(fn: (Int) -> Int) {
    lambdaList.add(fn)
}

fun main(args: Array<String>) {
    collectFn({ it * 2 })
    collectFn({ it * 3 })
    // 尾随闭包
    collectFn { it * it }
    collectFn { it * it * it }

    println(lambdaList.size)
    for (i in lambdaList.indices) {
        println(lambdaList[i](5))
    }
}
```

## 6.7 Lambda 表达式

### 6.7.1 调用 Lambda 表达式

```
package ch671

/**
 * 6.7.1 调用 Lambda 表达式
 */

fun main(args: Array<String>) {
    var square = { n: Int ->
        n * n
    }
    println(square(5))

    // 直接调用 Lambda 表达式
    var result = { base: Int, exponent: Int ->
        var result = 1
        for (i in 1..exponent) {
            result *= base
        }
        result
    }(2, 4)
    println(result)
}
```

### 6.7.2 利用上下文推断类型

```
package ch672

/**
 * 6.7.2 利用上下文推断类型
 */

fun main(args: Array<String>) {
    // 根据变量声明可以推断出 Lambda 表达式的形参类型
    var square: (Int) -> Int = { n -> n * n }
    println(square(5))

    var list = listOf("Java", "Kotlin", "Go")
    // 根据 dropWhile 的方法签名可以推断出 Lambda 表达式的形参类型
    var result1 = list.dropWhile { e -> e.length > 3 }
    println(result1)

    // 变量没有声明类型，无法推断 Lambda 表达式的形参类型。
    var result2 = { base: Int, exponent: Int ->
        var result = 1
        for (i in 1..exponent) {
            result *= base
        }
        result
    }(4, 3)
    println(result2)
}
```

### 6.7.3 省略形参名

```
package ch673

/**
 * 6.7.3 省略形参名
 */

fun main(args: Array<String>) {
    var square: (Int) -> Int = { it * it }
    println(square(5))

    var list = listOf("Java", "Kotlin", "Go")
    var result1 = list.dropWhile { it.length > 3 }
    println(result1)
}
```

### 6.7.4 调用 Lambda 表达式的约定

```
package ch674

/**
 * 6.7.4 调用 Lambda 表达式的约定
 */

fun main(args: Array<String>) {
    var list = listOf("Java", "Kotlin", "Go")
    var result1 = list.dropWhile({ it.length > 3 })
    // 最后一个参数是 Lambda 表达式，可将表达式写在括号外面。
    var result2 = list.dropWhile { it.length > 3 }
    println(result1)
    println(result2)
}
```

### 6.7.5 个数可变的参数和 Lambda 参数

```
package ch675

/**
 * 6.7.5 个数可变的参数和 Lambda 参数
 */

fun <T> test(vararg names: String, transform: (String) -> T): List<T> {
    var mutableList: MutableList<T> = mutableListOf()
    for (name in names) {
        mutableList.add(transform(name))
    }
    return mutableList.toList()
}

fun main(args: Array<String>) {
    var list1 = test("Java", "Kotlin", "Go") { it.length }
    println(list1)

    var list2 = test("Java", "Kotlin", "Go") { "Hello $it" }
    println(list2)
}
```

## 6.8 匿名函数

### 6.8.1 匿名函数的用法

```
package ch681

/**
 * 6.8.1 匿名函数的用法
 */

fun main(args: Array<String>) {
    var fun1 = fun(x: Int, y: Int): Int {
        return x + y
    }
    println(fun1(2, 4))

    // 匿名函数的函数体是单表达式，可以省略声明函数的返回值类型。
    var fun2 = fun(x: Int, y: Int) = x + y
    println(fun2(2, 4))

    // 使用匿名函数作为参数
    var filteredList1 = listOf(1, 3, 23, 45, 2).filter(
            fun(e): Boolean {
                return e > 20
            }
    )
    println(filteredList1)

    var filteredList2 = listOf(1, 3, 23, 45, 2).filter(
            fun(e): Boolean = e > 20
    )
    println(filteredList2)
}
```

### 6.8.2 匿名函数和 Lambda 表达式的 return

```
package ch68

/**
 * 6.8.2 匿名函数和 Lambda 表达式的 return
 */

fun test(fn: (Int) -> Unit) {
    fn(10)
}

fun main(args: Array<String>) {
    var list = listOf(3, 4, 45, 23)
    list.forEach(fun(n) {
        print("$n ")
        return // 匿名函数中的 return 返回该函数本身
    })
    
    list.forEach {
        println("$it ")
        return // Lambda 表达式中的 return 返回它所在的 main 函数（设想把 Lambda 表达式赋值给一个变量，该变量所在的函数就是 main 函数。）
    }

    test {
        println(it)
        // Lambda 表达式一般不允许使用 return，除非用在内联函数中，上面的 forEach 方法就是一个内联函数。
        // return
    }

    list.forEach {
        print("$it ")
        return@forEach // 限定返回
    }
}
```

## 6.9 捕获上下文中的变量和常量

```
package ch6_9

/**
 * 6.9 捕获上下文中的变量和常量
 */

fun makeList(e: String): () -> List<String> {
    var list: MutableList<String> = mutableListOf()
    fun addElement(): List<String> {
        // 可以访问宿主函数的变量（副本）和参数
        list.add(e)
        return list
    }
    return ::addElement
}

fun main(args: Array<String>) {
    // add1 和 add2 各自持有 list 的副本
    val add1 = makeList("Hello")
    println(add1())
    println(add1())
    val add2 = makeList("Kotlin")
    println(add2())
    println(add2())
}
```

## 6.10 内联函数

### 6.10.1 内联函数的使用

```
package ch6_10

/**
 * 6.10.1 内联函数的使用
 */

inline fun map(data: Array<Int>, fn: (Int) -> Int): Array<Int> {
    var result = Array<Int>(data.size, { 0 })
    for (i in data.indices) {
        result[i] = fn(data[i])
    }
    return result
}

fun main(args: Array<String>) {
    var arr = arrayOf(10, 20, 30, 40, 50)
    var mappedResult = map(arr, { it + 3 })
    println(mappedResult.contentToString())
}
```

### 6.10.2 部分禁止内联

```
package ch6_10

/**
 * 6.10.2 部分禁止内联
 */

inline fun test(fn1: (Int) -> Int, noinline fn2: (String) -> String) {
    println(fn1(20))
    println(fn2("Kotlin"))
}

fun main(args: Array<String>) {
    test({ it * it }, { "Hello $it" })
}
```

### 6.10.3 非局部返回

参考 6.8.2

```
package sec10

/**
 * 6.10.3 非局部返回
 */

inline fun each(data: Array<Int>, fn: (Int) -> Unit) {
    for (e in data) {
        fn(e)
    }
}

// 函数 each 添加了 inline 修饰符后等价于下面的代码
fun each1(data: Array<Int>) {
    for (e in data) {
        println(e)
        return
    }
}

fun main(args: Array<String>) {
    var arr = arrayOf(20, 4, 40, 100, 30)
    each(arr, {
        println(it)
        return
    })
}
```