# [Node.js REPL](http://www.runoob.com/nodejs/nodejs-repl.html)

启动交互终端
```bash
node
```

## 简单的表达式运算

```
> 1 + 4
5
> 5 / 2
2.5
> 3 * 6
18
> 4 - 1
3
> 1 + (2 * 3) - 4
3
```

## 使用变量

变量声明需要使用 `var` 关键字，如果没有使用 `var` 关键字变量会直接打印出来。
```
> x = 10
10
> var y = 10
undefined
> x + y
20
> console.log('Hello World')
Hello World
undefined
```

## 多行表达式

```
> var x = 0
undefined
> do {
... x++;
... console.log('x: ' + x);
... } while (x < 5);
x: 1
x: 2
x: 3
x: 4
x: 5
undefined
```

## 下划线 `_` 变量

你可以使用下划线 `_` 获取表达式的运算结果：
```
> var x = 10
undefined
> var y = 20
undefined
> x + y
30
> var sum = _
undefined
> console.log(sum)
30
undefined
```

## REPL 命令

```
> .help
.break    Sometimes you get stuck, this gets you out
.clear    Alias for .break
.editor   Enter editor mode
.exit     Exit the repl
.help     Print this help message
.load     Load JS from a file into the REPL session
.save     Save all evaluated commands in this REPL session to a file
```