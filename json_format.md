---
description: 本节介绍JSON的基本语法，能够帮助您更好得使用JSON编写简洁可靠的控制文件。当然，若您希望快速上手，后续再进行优化的话，建议您直接前往下一节。
---

# JSON基本语法

点击链接直接前往[下一节](control_file/)开始将CTL控制文件改写为JSON。

{% page-ref page="control\_file/" %}

## 概括

JSON作为数据交换语言十分便于阅读，只需要记住下面五点便可轻松阅读：

1. Data is in name/value pairs（基本元素以“名称-值”对的形式存在\`）；
2. Data is separated by commas（数据对之间以逗号分隔）；
3. Curly braces hold objects（{}内为对象）；
4. Square brackets hold arrays（\[\]内为数组）；
5. 值可以为对象或数组（可以嵌套）。

## 将JSON用于数据交换

### JSON建构于两种结构：

* **“名称/值”对的集合**（A collection of name/value pairs）。不同的语言中，它被理解为对象（object），纪录（record），结构（struct），字典（dictionary），哈希表（hash table），有键列表（keyed list），或者关联数组 （associative array）。 
* **值的有序列表**（An ordered list of values）。在大部分语言中，它被理解为数组（array）。 这些都是常见的数据结构。事实上大部分现代计算机语言都以某种形式支持它们。这使得一种数据格式在同样基于这些结构的编程语言之间交换成为可能。

### JSON具有以下这些形式：

对象（object）——是一个无序的“‘名称/值’对”集合。一个对象以“{”（左括号）开始，“}”（右括号）结束。每个“名称”后跟一个“:”（冒号）；“‘名称/值’ 对”之间使用“,”（逗号）分隔。

![Object&#xFF08;&#x5BF9;&#x8C61;&#xFF09;](.gitbook/assets/image%20%283%29.png)

数组（array）——是值（value）的有序集合。一个数组以“\[”（左中括号）开始，“\]”（右中括号）结束。值之间使用“,”（逗号）分隔。

![Array&#xFF08;&#x6570;&#x7EC4;&#xFF09;](.gitbook/assets/image%20%284%29.png)

值（value）——可以是双引号括起来的字符串（string）、数值\(number\)、`true`、`false`、 `null`、对象（object）或者数组（array）。这意味着JSON的结构可以嵌套。

![Value&#xFF08;&#x503C;&#xFF09;](.gitbook/assets/image%20%285%29.png)

字符串（_string_）——是由双引号包围的任意数量Unicode字符的集合，使用反斜线转义。一个字符（character）即一个单独的字符串（character string）。

![String&#xFF08;&#x5B57;&#x7B26;&#x4E32;&#xFF09;](https://www.json.org/string.gif)

数值（number）——除去未曾使用的八进制与十六进制格式以及一些编码细节与C或者Java的数值非常相似。

![Number&#xFF08;&#x6570;&#x503C;&#xFF09;](https://www.json.org/number.gif)

JSON对空白不敏感，空白可以加入到任何**符号**之间。

## Reference

1. [https://www.json.org/json-zh.html](https://www.json.org/json-zh.html)

