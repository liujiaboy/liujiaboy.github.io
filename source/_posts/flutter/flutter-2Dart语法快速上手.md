---
title: flutter-2Dart语法快速上手
date: 2022-12-27 11:48:08
tags:
    - flutter
categories:
    - flutter
---

## 一. 几个重要概念

在学习 Dart 语言时, 先要清楚以下几个事实和概念：

- Dart 语句是以分号 `;` 结尾的。
- 任何保存在变量中的都是一个对象，并且所有的对象都是对应一个类的实例。无论是数字，函数和 `null` 都是对象。所有对象继承自 Object 类。
- 尽管 Dart 是强类型的，但是 Dart 可以推断类型，所以类型注释是可选的。 如果要明确说明不需要任何类型， 需要使用特殊类型 dynamic 。
- Dart 支持泛型，如 `List <int>`（整数列表）或 `List <dynamic>`（任何类型的对象列表）。
- Dart 支持顶级函数，例如 `main()` ，同样函数绑定在类或对象上（分别是静态函数和实例函数）。以及支持函数内创建函数（ 嵌套或局部函数）。
- Dart 支持顶级变量，同样变量绑定在类或对象上（静态变量和实例变量）。实例变量有时称为字段或属性。
- 与 Java 不同，Dart 没有关键字 `public` ， `protected` 和 `private` 。如果标识符以下划线 `_` 开头，则它相对于库是私有的。
- 标识符以字母或下划线 `_` 开头，后跟任意字母和数字组合。

二. Dart关键字

Dart 语言关键字列表如下：

| else     | import   | static   | assert  | enum      |
| -------- | -------- | -------- | ------- | --------- |
| in       | uper     | async    | export  | interface |
| switch   | await    | extends  | is      | sync      |
| break    | external | library  | this    | case      |
| factory  | mixin    | throw    | catch   | false     |
| new      | true     | class    | final   | null      |
| try      | const    | finally  | on      | typedef   |
| continue | for      | operator | var     | covariant |
| Function | part     | void     | default | get       |
| rethrow  | while    | deferred | hide    | return    |
| with     | do       | if       | set     | yield     |

使用 Dart 时应避免使用这些单词作为标识符。

## 三. 入口函数

Dart 有两种入口主函数：

```
 main(){
  print('Hello Dart!');
}

// 加 void 表示没有返回值
void main(){
  print('Hello Flutter!');
}
```

## 四. 代码注释

Dart 支持单行注释、多行注释和文档注释：

1. 单行注释：单行注释以 `//` 开始。 所有在 `//` 和改行结尾之间的内容被编译器忽略。
2. 多行注释：多行注释以 `/*` 开始， 以 `*/` 结尾。 所有在 `/*` 和 `*/` 之间的内容被编译器忽略 （不会忽略文档注释）。 多行注释可以嵌套。
3. 文档注释：文档注释可以是多行注释，也可以是单行注释，文档注释以  `///` 或者 `/**` 开始。 在连续行上使用 `///` 与多行文档注释具有相同的效果。

```
// 单行注释
/* 
 * 多行注释
 */ 

/// 文档注释

/** 
 * 文档注释
 */三、变量
```

## 五. 变量

**1. 定义变量**

Dart 中定义变量有两种方式，一种是静态类型语言常用的方式，即指定变量类型；另一种则是动态语言的常用方式，即不指定类型，由 Dart 自动类型判断，一般最好指定类型。

 `// 指定变量类型：`

```
String name = 'ImportV';

// 使用关键字 var ，不指定变量类型：
var name = 'ImportV';
```

未初始化的变量默认值是 `null`。即使变量是数字类型默认值也是 `null`，因为在 Dart 中一切都是对象，数字类型也不例外。

```
int a;
print(a);  // 输出值为 null
```

\2. 动态改变变量类型

如想动态改变变量的数据类型，应当使用 `dynamic` 或 `Object` 来定义变量：

```
dynamic a = 'Dart';  // 也可使用 Object a = 'Dart';
a = 2021;
print(a);  // 输出结果为 2021
```

\3. 定义常量

Dart 中定义常量也有两种方式，一种使用 `final` 关键字，另一种是使用 `const` 关键字。需要注意的是， `final` 定义的常量是运行时常量，而 `const` 常量则是编译时常量，也就是说 `final` 定义常量时，其值可以是一个变量，而 `const` 定义的常量，其值必须是一个字面常量值。

```
final a = DateTime.now();
print(a);  // 输出现在的时间

const a = DateTime.now();
print(a);  // 报错

六. 内建类型
```

Dart 语言支持以下内建类型：


![img](http://space.babytree-inc.com/download/attachments/30609176/640.png?version=2&modificationDate=1645091354000&api=v2)
\1. Number

```
// int 必须是整型：
int a = 123;

// 从 Dart 2.0 开始，double 既可以是浮点数也可以是整型：
double b = 12.34;
double c = 6;    // 相当于 double c = 6.0
```

\2. String

```
// 用单引号或者双引号定义字符串：
String s1 = 'Hello Dart!'
String s2 = "Hello Flutter!"

// 当字符串有引号时：
String s3 = 'I\'m ImportV!'
string s4 = "I'm ImportV!"

// 使用连续三个单引号或者三个双引号实现多行字符串对象的创建：
String s5 = '''
Hello Dart! 
Hello Flutter! 
Hello ImportV!
''';
// 使用 r 前缀，可以创建 “原始 raw” 字符串：
String s6 = r'Hello Dart \n Hello Flutter!'

// String -> int：
int a = int.parse('123');

// String -> double：
double b = double.parse('12.34');

// int -> String：
String str1 = 123.toString();

// double -> String：
String str2 = 12.3456.toStringAsFixed(2);  // 括号内为要保留的小数位数

// 字符串拼接，用“+”拼接，字面量字符串也可以直接写在一起：
String s1 = 'Hello Dart! ' 'Hello Flutter! ' 'Hello ImportV! ';
String s2 = 'Hello Dart! ' + 'Hello Flutter! ' + 'Hello ImportV! ';

// 字符串可以通过 ${expression} 的方式内嵌表达式。 如果表达式是一个标识符，则 {} 可以省略:
String s1 = 'Hello Flutter!';
String s2 = 'Hello Dart! $s1 Hello ImportV!';
```

\3. Boolean

```
// 布尔类型默认值为null，字面量只有 true 和 false ，这两个对象都是编译时常量：
bool b = true;
```

\4. List

```
// 定义列表方法一：
List lt1 = [1, 2, 3, 4];

// 定义列表方法二：
List a = [];
a.add(1);
a.add(2);
a.add(3);

// 获取列表属性：
List a = ['Dart', 'Flutter', 'ImportV'];
print(a.length);  // 获取列表长度
print(a.isEmpty);  // 判断列表是否为空，如果是，输出 true
print(a.isNotEmpty);  // 判断列表是否为空，如果不是，输出 true
print(a.reversed);  // 将列表逆序输出（翻转列表）

// 列表的下标索引是从0开始的：
List a = [1, 2, 3, 4];
print(a[0]);

// 拼接与插入：
List a = ['Dart', 'Flutter'];
a.add('Android');  // 拼接一个元素
a.addAll(['Android', 'iOS']);  // 拼接多个元素
a.insert(2, 'Android');  // 插入一个元素，2 为插入位置的索引值
a.insertAll(2, ['Android', 'iOS']);  // 插入多个元素

// 列表转字符串：
List a = ['Dart', 'Flutter', 'Android'];
String b = a.join('->');  // 此处 b 的字面量为 Dart->Flutter->Android

// 字符串转列表：
String a = 'Dart->Flutter->Android';
List b = a.split('->');  // 此处 b 的字面量为 [Dart, Flutter, Android]

// 指定列表类型：
List a = <String>[];
a.add('Flutter');
a.add(1);  //报错

// 在列表字面量前添加 const 关键字，定义一个不可改变的列表（编译时常量）：
List a = const [1, 2, 3, 4];
a[0] = 2;  // 报错
```

\5. Set

Set 是没有顺序且元素不能重复的集合，因此不能通过索引去获取值。

```
// 定义集合方法一：
Set a = {'Dart', 'Flutter', 'ImportV'};

// 定义集合方法二
Set a = {};
a.add('Dart');
a.add('Flutter');
a.add('ImportV');

// 创建空集合：
Set a = {};  //  方法一
var a = <String>{};  // 方法二
var a = {};  // 这样会创建一个空的 Map，而不是 Set

// 获取集合中元素的个数：
Set a = {'Dart', 'Flutter', 'ImportV'};
print(a.length);  // 输出为 3

// 创建一个不可改变的集合（编译时常量）：
Set a = const {'Dart', 'Flutter', 'ImportV'};
a.add('Android');  // 报错

// 列表转集合：
List a = ['Dart', 'Flutter', 'Android'];
print(a.toSet());

// 集合转列表：
Set a = {'Dart', 'Flutter', 'Android'};
print(a.toList());
```

\6. Map

Map（映射）是用来关联 keys 和 values 的对象。 keys 和 values 可以是任何类型的对象。在一个 Map 对象中一个 key 只能出现一次。 但是 value 可以出现多次。

```
// 定义映射方法一：
Map a = {
// key     value
  'first': 'Dart',
  'second': 'Flutter',
  'third': 'Android'
  };

// 定义映射方法二：
Map a = {};
a['first'] = 'Dart';
a['second'] = 'Flutter';
a['third'] = 'Android';

// 获取映射长度：
print(a.length);

// 获取指定 value 值：
print(a['first']);

// 在映射字面量前添加 const 关键字，定义一个不可改变的映射（编译时常量）：
Map a = const {'first': 'Dart', 'second': 'Flutter', 'third': 'Android'};
a['third'] = 'iOS'  //报错五、运算符
```

## 七. 运算符

下面是 Dart 定义的运算符：

**1. 算数运算符**

除了基本的算术运算外，Dart 还支持前缀和后缀、自增和自减运算符：

```
var a, b;
a = 0;

b = ++a;  // a自加1后赋值给b
b = a++;  // a赋值给b后自加1
b = --a;  // a自减1后赋值给b
b = a--;  // a赋值给b后自减1
```

### 2. 类型判断运算符

使用 `as` 运算符将对象强制转换为特定类型时，通常可以认为是 `is` 类型判定后，被判定对象调用函数的一种缩写形式。 请考虑以下代码：

```
if (emp is Person) {
  emp.firstName = 'Bob';
}
```

使用 as 运算符进行缩写：

```
(emp as Person).firstName = 'Bob';
```

【注意】以上代码并不是等价的。 如果 `emp` 为 `null` 或者不是 `Person` 对象， 那么第一个 `is` 的示例，后面将不执行； 第二个 `as` 的示例会抛出异常。

### 3. 赋值运算符

下面示例使用几个赋值和复合赋值运算符，其他使用方法类似：

```
a = 123;  // 使用 = 直接为变量赋值。

b ??= 456;  // 使用 ??= 运算符时，只有当 b 值为 null 时才会被赋值

a *= 3;  // 赋值并做乘法运算，相当于 a = a * 3
```

### 4. 逻辑运算符

下面是关于逻辑表达式的示例：

```
if (!done && (col == 0 || col == 3)) {
  // ...Do something...
}
```

### 5. 位运算符

位运算符把数字转为二进制来运算：

```
// 按位与，参与运算的两个值，如果两个相应二进位都为 1，则该位结果为 1，否则为 0
print(a & b);  // 结果为 4

// 按位或，只要对应的两个二进位有一个为 1，结果位就为 1
print(a | b);  // 结果为 14

// 按位异或，当两个对应的二进位相异时，结果位为 1，否则为 0
print(a ^ b);  // 结果为 10

// 按位取反，对数据的每一个二进位，把 1 变为 0，把 0 变为 1
print(~a);  // 结果我为 -13

// 左移，把 << 左边的数的各二进位左移若干位，由 << 右边的数指定移动位数，高位丢弃，低位补 0
print(a << 2);  // 结果为 48，相当于乘以 2 的 2 次方

// 右移，把 << 左边的数的各二进位右移若干位，由 << 右边的数指定移动位数
print(a >> 2);  // 结果为 3，相当于除以 2 的 2 次方
```

### 6. 条件表达式

Dart有两个运算符，有时可以替换 if-else 表达式， 让表达式更简洁，称为条件表达式：

```
condition ? expr1 : expr2
```

如果条件为 `true`, 执行 `expr1` (并返回它的值)： 否则, 执行并返回 `expr2` 的值。

```
expr1 ?? expr2
```

如果 `expr1` 不是 `null`， 返回 `expr1` 的值； 否则, 执行并返回 `expr2` 的值。

如果条件判断是根据布尔值， 考虑使用第一种，如果是根据是否为 `null`，考虑使用第二种。

```
var a = 'Dart';
var b = 'Flutter';
var c = a ?? b.toUpperCase();  // 此处 c 的字面量为 Dart
```

### 7. 级联运算符

级联运算符 `..` 可以实现对同一个对像进行一系列的操作。 除了调用函数外， 还可以访问同一对象上的字段属性。 这通常可以节省创建临时变量的步骤， 同时编写出更流畅的代码。

```
querySelector('#confirm')   // 获取对象。
  ..text = 'Confirm'    // 调用成员变量。
  ..classes.add('important')
  ..onClick.listen((e) => window.alert('Confirmed!'));
```

第一句调用函数 `querySelector()` ，返回获取到的对象。 获取的对象依次执行级联运算符后面的代码， 代码执行后的返回值会被忽略。

上面的代码等价于：

```
var button = querySelector('#confirm');
button.text = 'Confirm';
button.classes.add('important');
button.onClick.listen((e) => window.alert('Confirmed!'));
```

级联运算符是可以嵌套的。但要注意，在返回对象的函数中谨慎使用级联操作符。 例如，下面的代码是错误的：

```
var sb = StringBuffer();
sb.write('foo')
  ..write('bar');   // sb.write() 函数调用返回 void， 不能在 void 对象上创建级联操作。六、流程控制语句
```

##  

## 八. 流程控制语句

Dart 中的流程控制语句与 Java 相似。

**1. if - else 分支**

和 JavaScript 不同， Dart 的判断条件必须是布尔值，不能是其他类型。

```
int a = 1;
if (a < 0) {
  a++;
} else if (a > 0) {
  a--;
}
```

### 2. switch - case 语句

在 Dart 中 `switch` 语句使用 `==` 比较整数、字符串、编译时常量或者枚举类型。 比较的对象必须都是同一个类的实例（并且不可以是子类）， 类必须没有对 `==` 重写。

在 `case` 语句中，每个非空的 `case` 语句结尾需要跟一个 `break` 语句。 除  `break`以外，还有可以使用 `continue`， `throw` 或者 `return`。当没有 `case` 语句匹配时，执行 `default` 代码：

```
var a = 'yes';
switch (a) {
  case 'yes':
    print('yes');
    break;
  case 'no':
    print('no');
    break;
  default:
    print('fault');
}
```

### 3. for 循环

进行迭代操作，可以使用标准 for 语句。 例如：

```
// 计算 5 的阶乘
int result = 1;
for (int i = 1; i < 6; i++) {
  result = result * i;
}
```

闭包在 Dart 的 for 循环中会捕获循环的初始索引值， 来避免 JavaScript 中常见的陷阱。下面的代码输出的是 0 和 1，但是在 JavaScript 中会连续输出两个 2 ：

```
var callbacks = [];
for (var i = 0; i < 2; i++) {
  callbacks.add(() => print(i));
}
callbacks.forEach((c) => c());
```

### 4. while 和 do while 循环

`while` 循环是在执行前判断执行条件，`do-while` 循环是在执行后判断执行条件：

```
// 计算 5 的阶乘
int i = 1;
int result = 1;
// 使用 while 循环
while (i < 6) {
  result = result * i;
  i++;
}
// 使用 do - while 循环
do {
  result = result * i;
  i++;
} while (i < 6);
```

### 5. break 和 continue

使用 `break` 停止程序循环：

```
for (int i = 1; i < 5; i++) {
  if (i == 3) {
    break;
  }
  print(i);  // 输出结果为 1 2（i = 3 时循环结束）
}
```

使用 `continue` 跳转到下一次循环：

```
for (int i = 1; i < 5; i++) {
  if (i == 3) {
    continue;
  }
  print(i);  // 输出结果为 1 2 4（i = 3 时跳到下一次循环）
}
```

### 6. 其他循环

使用 `for...in...` 循环：

```
// 遍历数组
List a = ['Dart', 'Flutter', 'Android'];
for (var item in a) {
  print(item);
}
```

使用 `forEach` 循环：

```
// 遍历数组
List a = ['Dart', 'Flutter', 'Android'];
a.forEach((var item) {
  print(item);
});
```

### 7. assert 语句

使用 `assert` 语句进行判断，如果 `assert` 语句中的布尔条件为 `false` ， 那么正常的程序执行流程会被中断:

```
// 确认变量值不为空。
assert(text != null);

// 确认变量值小于100。
assert(number < 100);
```

【注意】`assert` 语句只在开发环境中有效， 在生产环境是无效的； Flutter 中的 `assert` 只在 debug 模式中有效。

##  

## 九. 异常处理

Dart 代码可以抛出和捕获异常。和 Java 有所不同， Dart 中的所有异常是非检查异常。方法不会声明它们抛出的异常，也不要求捕获任何异常。

Dart 提供了 `Exception` 和 `Error` 类型， 以及一些子类型，也可以定义自己的异常类型。Dart 程序可以抛出任何非 `null` 对象， 不仅限 `Exception` 和 `Error` 对象。

### 1. throw 抛出异常

下面是关于抛出或者引发异常的示例：

```
throw FormatException('Expected at least 1 section');
```

也可以抛出任意的对象：

```
throw 'Out of llamas!';
```

### 2. try...catch...finally 捕获异常

Dart 语言中通过指定多个 `catch` 语句，可以处理可能抛出多种类型异常的代码，并由与抛出异常类型匹配的第一个 `catch` 语句处理异常。 如果 `catch` 语句未指定类型， 则该语句可以处理任何类型的抛出对象。

此外，`catch()` 函数可以指定1到2个参数， 第一个参数为抛出的异常对象， 第二个为堆栈信息。

使用 `finally` 语句时，无论是否抛出异常，`finally` 中的代码都会被执行。

示例代码如下：

```
try {
  print(10 ~/ 0);
} on IntegerDivisionByZeroException catch (e, s) {
  print(e);
  print(s);
} on Exception catch (e, s) {
  print(e);
  print(s);
}
```

执行以上代码将输出：

```
// 捕获的异常
IntegerDivisionByZeroException  
// 堆栈信息
#0      int.~/ (dart:core-patch/integers.dart:22:7)  
#1      main (file:///c:/Users/27884/Desktop/test.dart:3:14)
#2      _startIsolate.<anonymous closure> (dart:isolate-patch/isolate_patch.dart:301:19)
#3      _RawReceivePortImpl._handleMessage (dart:isolate-patch/isolate_patch.dart:168:12)
// finally 语句后的代码执行结果
end
```
