[TOC]

# 第 6 章 PHP 的流程控制功能

## 6.1 转换流程

### 6.1.1 `if` 语句

**程序清单 6.1 `if` 语句**
```php
<?php
$mood = "happy";
if ($mood == "happy") {
    echo "Hooray! I'm in a good mood!";
}
?>
```

输出结果：

Hooray! I'm in a good mood!

### 6.1.2 使用 `else` 子句的 `if` 语句

**程序清单 6.2 使用 `else` 的 `if` 语句**
```php
<?php
$mood = "sad";
if ($mood == "happy") {
    echo "Hooray! I'm in a good mood!";
} else {
    echo "I'm in a $mood mood.";
}
?>
```

输出结果：

I'm in a sad mood.

### 6.1.3 使用带有 `elseif` 子句的 `if` 语句

**程序清单 6.3 使用 `else` 和 `elseif` 的 `if` 语句**
```php
<?php
$mood = "sad";
if ($mood == "happy") {
    echo "Hooray! I'm in a good mood!";
} elseif ($mood == "sad") {
    echo "Awww. Don't be down!";
} else {
    echo "I'm neither happy nor sad, but $mood.";
}
?>
```

输出结果：

I'm in a sad mood.

### 6.1.4 `switch` 语句

**程序清单 6.4 `switch` 语句**
```php
<?php
$mood = "sad";
switch ($mood) {
    case "happy":
        echo "Hooray! I'm in a good mood!";
        break;
    case "sad":
        echo "Awww. Don't be down!";
        break;
    default:
        echo "I'm neither happy nor sad, but $mood.";
        break;
}
?>
```

输出结果：

Awww. Don't be down!

### 6.1.5 使用 `?` 运算符

**程序清单 6.5 使用 `?` 运算符**
```php
<?php
$mood = "sad";
$text = ($mood == "happy") ? "I am in a good mood!" : "I am in a $mood mood.";
echo "$text";
?>
```

输出结果：

I am in a sad mood.

## 6.2 循环

### 6.2.1 `while` 语句

**程序清单 6.6 `while` 语句**
```php
<?php
$counter = 1;
while ($counter <= 12) {
    echo $counter." times 2 is ".($counter * 2)."<br/>";
    $counter++;
}
?>
```

输出结果：

1 times 2 is 2<br/>2 times 2 is 4<br/>3 times 2 is 6<br/>4 times 2 is 8<br/>5 times 2 is 10<br/>6 times 2 is 12<br/>7 times 2 is 14<br/>8 times 2 is 16<br/>9 times 2 is 18<br/>10 times 2 is 20<br/>11 times 2 is 22<br/>12 times 2 is 24<br/>

### 6.2.2 `do...while` 语句

**程序清单 6.7 `do...while` 语句**
```php
<?php
$num = 395;
do {
    echo "The number is: ".$num."<br/>";
    $num++;
} while ( ($num > 200) && ($num < 400) );
?>
```

输出结果：

The number is: 395<br/>The number is: 396<br/>The number is: 397<br/>The number is: 398<br/>The number is: 399<br/>

### 6.2.3 `for` 语句

**程序清单 6.8 使用 `for` 语句**
```php
<?php
for ($counter = 1; $counter <= 12; $counter++) { 
    echo $counter." times 2 is ".($counter * 2)."<br/>";
}
?>
```

输出结果：

1 times 2 is 2<br/>2 times 2 is 4<br/>3 times 2 is 6<br/>4 times 2 is 8<br/>5 times 2 is 10<br/>6 times 2 is 12<br/>7 times 2 is 14<br/>8 times 2 is 16<br/>9 times 2 is 18<br/>10 times 2 is 20<br/>11 times 2 is 22<br/>12 times 2 is 24<br/>

### 6.2.4 用 `break` 语句跳出循环

**程序清单 6.10 使用 `break` 语句**
```php
<?php
for ($counter = -4; $counter <= 10; $counter++) { 
    if ($counter == 0) {
        break;
    }
    $temp = 4000/$counter;
    echo "4000 divided by ".$counter." is $temp<br/>";
}
?>
```

输出结果：

4000 divided by -4 is -1000<br/>4000 divided by -3 is -1333.33333333<br/>4000 divided by -2 is -2000<br/>4000 divided by -1 is -4000<br/>

**提示：在 PHP 里用一个数除以零不会导致一个致命的错误。相反，PHP 产生一个警告并继续执行。**

```php
<?php
$result = 10/0;
echo "$result";
?>
```

输出结果：

<br />
<b>Warning</b>:  Division by zero in <b>/Applications/MAMP/htdocs/example.php</b> on line <b>2</b><br />

### 6.2.5 用 `continue` 语句跳过迭代

**程序清单 6.11 使用 `continue` 语句**
```php
<?php
for ($counter = -4; $counter <= 10; $counter++) { 
    if ($counter == 0) {
        continue;
    }
    $temp = 4000/$counter;
    echo "4000 divided by ".$counter." is $temp<br/>";
}
?>
```

输出结果：

4000 divided by -4 is -1000<br/>4000 divided by -3 is -1333.33333333<br/>4000 divided by -2 is -2000<br/>4000 divided by -1 is -4000<br/>4000 divided by 1 is 4000<br/>4000 divided by 2 is 2000<br/>4000 divided by 3 is 1333.33333333<br/>4000 divided by 4 is 1000<br/>4000 divided by 5 is 800<br/>4000 divided by 6 is 666.666666667<br/>4000 divided by 7 is 571.428571429<br/>4000 divided by 8 is 500<br/>4000 divided by 9 is 444.444444444<br/>4000 divided by 10 is 400<br/>

### 6.2.6 嵌套循环

**程序清单 6.12 嵌套两个 `for` 循环**
```php
<?php
echo "<table style=\"border: 1px solid #000\">\n";
for ($y = 1; $y <= 12; $y++) {
    echo "<tr>\n";
    for ($x = 1; $x <= 12; $x++) {
        echo "<td style=\"border: 1px solid #000; width: 75px; text-align: center;\">";
        echo ($y * $x);
        echo "</td>\n";
    }
    echo "</tr>\n\n";
}
echo "</table>";
?>
```

输出结果：

<table style="border: 1px solid #000">
<tr>
<td style="border: 1px solid #000; width: 75px; text-align: center;">1</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">2</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">3</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">4</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">5</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">6</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">7</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">8</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">9</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">10</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">11</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">12</td>
</tr>

<tr>
<td style="border: 1px solid #000; width: 75px; text-align: center;">2</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">4</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">6</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">8</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">10</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">12</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">14</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">16</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">18</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">20</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">22</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">24</td>
</tr>

<tr>
<td style="border: 1px solid #000; width: 75px; text-align: center;">3</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">6</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">9</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">12</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">15</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">18</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">21</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">24</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">27</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">30</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">33</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">36</td>
</tr>

<tr>
<td style="border: 1px solid #000; width: 75px; text-align: center;">4</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">8</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">12</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">16</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">20</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">24</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">28</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">32</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">36</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">40</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">44</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">48</td>
</tr>

<tr>
<td style="border: 1px solid #000; width: 75px; text-align: center;">5</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">10</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">15</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">20</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">25</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">30</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">35</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">40</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">45</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">50</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">55</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">60</td>
</tr>

<tr>
<td style="border: 1px solid #000; width: 75px; text-align: center;">6</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">12</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">18</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">24</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">30</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">36</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">42</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">48</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">54</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">60</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">66</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">72</td>
</tr>

<tr>
<td style="border: 1px solid #000; width: 75px; text-align: center;">7</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">14</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">21</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">28</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">35</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">42</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">49</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">56</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">63</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">70</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">77</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">84</td>
</tr>

<tr>
<td style="border: 1px solid #000; width: 75px; text-align: center;">8</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">16</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">24</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">32</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">40</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">48</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">56</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">64</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">72</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">80</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">88</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">96</td>
</tr>

<tr>
<td style="border: 1px solid #000; width: 75px; text-align: center;">9</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">18</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">27</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">36</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">45</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">54</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">63</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">72</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">81</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">90</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">99</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">108</td>
</tr>

<tr>
<td style="border: 1px solid #000; width: 75px; text-align: center;">10</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">20</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">30</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">40</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">50</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">60</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">70</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">80</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">90</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">100</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">110</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">120</td>
</tr>

<tr>
<td style="border: 1px solid #000; width: 75px; text-align: center;">11</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">22</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">33</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">44</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">55</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">66</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">77</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">88</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">99</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">110</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">121</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">132</td>
</tr>

<tr>
<td style="border: 1px solid #000; width: 75px; text-align: center;">12</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">24</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">36</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">48</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">60</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">72</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">84</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">96</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">108</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">120</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">132</td>
<td style="border: 1px solid #000; width: 75px; text-align: center;">144</td>
</tr>

</table>

## 6.3 代码块和浏览器输出

**程序清单 6.14 返回代码块中的 HTML 模式**
```html
<?php
$display_prices = true;
if ($display_prices) {
?>
<table border="1">
    <tr>
        <td colspan="3">today's prices in dollars</td>
    </tr>
    <tr>
        <td>$14.00</td>
        <td>$32.00</td>
        <td>$71.00</td>
    </tr>
</table>
<?php
}
?>
```

输出结果：

<table border="1">
    <tr>
        <td colspan="3">today's prices in dollars</td>
    </tr>
    <tr>
        <td>$14.00</td>
        <td>$32.00</td>
        <td>$71.00</td>
    </tr>
</table>

学习结束

复习 2018.2.26