[TOC]

# 第 2 章　Lambda表达式

## 2.1　第一个Lambda表达式

**例2-1　使用匿名内部类将行为和按钮单击进行关联**
```java
button.addActionListener(new ActionListener() {
    public void actionPerformed(ActionEvent event) {
        System.out.println("button clicked");
    }
});
```

**例2-2　使用Lambda表达式将行为和按钮单击进行关联**
```java
button.addActionListener(event -> System.out.println("button clicked"));
```

## 2.2　如何辨别Lambda表达式

**例2-3　编写Lambda表达式的不同形式**
```java
Runnable noArguments = () -> System.out.println("Hello World");

ActionListener oneArgument = event -> System.out.println("button clicked");

Runnable multiStatement = () -> {
    System.out.print("Hello");
    System.out.println(" World");
};

BinaryOperator<Long> add = (x, y) -> x + y;

BinaryOperator<Long> addExplicit = (Long x, Long y) -> x + y;
```

**目标类型**是指Lambda表达式所在上下文环境的类型。比如，将Lambda表达式赋值给一个局部变量，或传递给一个方法作为参数，局部变量或方法参数的类型就是Lambda表达式的目标类型。

## 2.3　引用值，而不是变量

**例2-5　匿名内部类中使用final局部变量**
```java
final String name = getUserName();
button.addActionListener(new ActionListener() {
    public void actionPerformed(ActionEvent event) {
        System.out.println("hi " + name);
    }
});
```

**例2-6　Lambda表达式中引用既成事实上的final变量**
```java
String name = getUserName();
button.addActionListener(event -> System.out.println("hi " + name));
```

**例2-7　未使用既成事实上的final变量，导致无法通过编译**
```java
String name = getUserName();
name = formatUserName(name);
button.addActionListener(event -> System.out.println("hi " + name));
```

## 2.4　函数接口

函数接口是只有一个抽象方法的接口，用作Lambda表达式的类型。

**例2-8　ActionListener接口：接受ActionEvent类型的参数，返回空**
```java
public interface ActionListener extends EventListener {
    public void actionPerformed(ActionEvent event);
}
```
这就是函数接口，接口中单一方法的命名并不重要，只要方法签名和Lambda表达式的类型匹配即可。可在函数接口中为参数起一个有意义的名字，增加代码易读性，便于更透彻地理解参数的用途。

## 2.5　类型推断

Lambda表达式中的类型推断，实际上是Java 7就引入的目标类型推断的扩展。读者可能已经知道Java 7中的菱形操作符，它可使javac推断出泛型参数的类型。参见例2-9。

**例2-9　使用菱形操作符，根据变量类型做推断**
```java
Map<String, Integer> oldWordCounts = new HashMap<String, Integer>();
Map<String, Integer> diamondWordCounts = new HashMap<>();
```

**例2-11　类型推断**
```java
Predicate<Integer> atLeast5 = x -> x > 5;
```

**例2-13　略显复杂的类型推断**
```java
BinaryOperator<Long> addLongs = (x, y) -> x + y;
```

学习结束