+++
title = '输入输出'
date = 2024-04-04T13:59:10+08:00
author = "Yotom"
description = "C++的基础——输入输出"
tags = [
    "IO",
    "输入输出",
    "注释",
	"不定数据输入",
]
categories = [
    "C/C++",    
    "学习记录",
]

+++

# 输入输出

C++采用了一个全面的**标准库**来提供**IO机制**。

**iostream**库:包含两个基础类型**istream**、**ostream**，分别用来表示输入流和输出流。一个流就是一个字符序列，从IO设备读出或者写入IO设备的。

**“流”（stream）**：表达的是，随着时间的推移，字符是顺序生成或消耗的。

## 标准输入输出对象

标准库定义了4个IO对象。

+ **cin**：发音为see-in，**istream**类型的对象，称作**标准输入**。
+ **cout**：发音为see-out，**ostream**类型的对象，称作**标准输出**。
+ **cerr**：发音为see-err，用于输出警告和错误信息，称作**标准错误**。
+ **clog**：发音为see-log，用于输出程序运行时的一般信息。

系统通常将程序和所运行的窗口与这些对象关联起来。因此，读取**cin**时，数据将从程序正在运行的窗口读入，向**cout**、**cerr**、**clog**写入数据时，将会写到同一个窗口。

---

## 向流写入数据

```c++
std::cout << "Enter two numbers:" << std::endl;
```

这是条语句执行了一个**表达式（expression）**。C++中表达式会产生一个计算结果，由一个或者多个运算对象和（通常有）一个运算符组成。这条语句中的表达式使用了**输出运算符（<<）**。

**<<运算符**接受两个运算对象：

1. 左侧的运算对象必须是一个**ostream**对象。
2. 右侧的运算对象必须是要打印的值。

上方语句中**endl**是一个被称作**操作符（manipulater）**的特殊值。效果是结束当前行，并将与设备关联的**缓冲区（buffer）**中的内容刷到设备中。缓冲刷新操作可以保证到目前为止程序所产生的所有输出都真正写入输出流中，而不是仅停留在内存中等待写入流。

---

## 命名空间

阅读上方代码时，可能会注意到这里使用了`std::cout`、`std::endl`，而不是直接使用`cout`、`endl`。前缀`std::`指出`cout`、`endl`是定义在名为**std**的**命名空间（namespace）**中的。命名空间可以帮助我们避免不经意的名字定义冲突，以及使用库中相同名字导致的冲突。标准库定义的所有名字都在命名空间std中。

但这有个副作用，当使用库中一个名字时，必须显式说明想要使用来自命名空间std中的名字。如`std::cout`。要通过**作用域运算符（::）**来指出我们想使用定义在命名空间std中的名字`cout`。相信熟悉C++的朋友已经知道，如果想要避免每次都使用`std::cout`这样的形式来调用`cout`，可以在头文件下方使用这样的代码段：

```c++
using namespace std;
```

在有这条代码段后，所有需要声明`std::`的名字，就可以省略掉这个前缀了。

---

## 从流读取数据

读取数据首先需要定义**变量（variable）**：

```c++
int a = 0, b = 0;
```

然后使用**输入运算符（>>）**和`std::cin`将输入流中的数据存储在变量中：

```c++
std::cin >> a >> b;
```

输入运算符>>规则与前文提到的输出运算符<<规则相似，这里不过赘述。

---

## 注释

c++中注释主要使用两种方法：

1. 单行注释使用`//`
2. 多行注释使用`/*`和`*/`来完成

例如：

```c++
//这里使用单行注释
/*
	这里是多行注释
	只需要把内容包含在两个符号之间
*/
int a, b;//定义变量a和b。
/*
下方提示用户输入变量a和b的值
并将求和结果输出
*/
cin >> a >> b;
cout << a+b;
```

---

## 读取数量不定的输入数据

### 给定输入数据数值n

如果我们给出一组数的具体数目，然后输入这组数，输出他们的和，这是个简单的问题：

```c++
int sum = 0, n, v;
cin >> n;//输入这组数据的具体数目
//通过while循环n次
while(n--){
    cin >> v;//每次输入当前的数据
    sum += v;//每次累加当前数据
}
cout << sum;//输出结果即可
```

### 无输入数据的数值

但如果我们不给出这个n，那还有办法可以完成吗？

```c++
int sum = 0, v;
while(cin >> v) sum += v;//读取数据到文件尾，计算所有输入值的和
cout << sum;
```

这样就可以实现只要不停止输入，就可以累加数值了。

### 结束输入

需要注意的是，当我们使用一个istream对象作为条件时，效果是检测流的状态。如果流是有效的，那么检测成功，而如果遇到**文件结束符（end-of-file）**，或者遇到一个无效输入的时候（例如不是整数类型），istream对象的状态就会无效，最终退出循环。

结束键盘输入的方式在不同系统中是不一样的。在**Windows**中，是**Ctrl+Z**然后按**Enter**或者**Return**。而在UNIX（包括MAC OS X）中，是使用**Ctrl+D**。

这一部分是控制输入输出的基础部分，有许多题目中可能会用到，但是在笔者观察下，很多同学是没有刻意去了解这部分内容的，作为C++基础重新巩固的环节，这部分内容必不可少。

---

## 文件重定向

当测试程序的时候，反复从键盘输入数据是非常乏味、枯燥的。大多数操作系统支持文件重定向，这种机制允许将标准输入和标准输出与命名文件关联起来：

`addItems <infile >outfile`

假定我们的程序已经编译为addItems.exe的可执行文件（UNIX中是addItems），则上述命令会从一个名为infile的文件中读取数据，并将结果写入一个名为outfile的文件中，两个文件都在当前目录中。

---

## 总结

这部分内容主要来源于C++ primer，比较基础，是我重学C++过程中前面的一小块，后面写的帖子应该着重于结合面经和自身缺漏来写。

---
