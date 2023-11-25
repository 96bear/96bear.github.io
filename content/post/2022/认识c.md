---
title: "认识C#"
date: "2022-04-24"
categories: 
  - "c"
  - "windows"
  - "app"
  - "zatan"
  - "编程"
---

# 初识C Sharp

## 历史

C#（读作“See Sharp”）是一种新式编程语言，不仅面向对象，还类型安全。 开发人员利用 C# 能够生成在 .NET 中运行的多种安全可靠的应用程序。 C# 源于 C 语言系列，C、C++、Java 和 JavaScript 程序员很快就可以上手使用。

## 特点

- 是通用编程语言
    
- 面向组件
    
- 产生高效率的程序
    
- 可在多种计算机平台上编译
    
- _C# 是面向对象的、面向组件的编程语言。你需要定义类型及其行为。_
    
- _多项 C# 功能有助于创建可靠且持久的应用程序。（异常处理）_
    
- _C# 强调版本控制，以确保程序和库以兼容方式随时间推移而变化。_
    
- _所有 C# 类型（包括 `int` 和 `double` 等基元类型）均继承自一个根 `object` 类型。 所有类型共用一组通用运算。 任何类型的值都可以一致地进行存储、传输和处理。_
    
- _C# 可提供迭代器_
    
- _C# 允许动态分配轻型结构的对象和内嵌存储。_
    
- _语言集成查询 (LINQ) 语法创建一个公共模式，用于处理来自任何源的数据。_
    
- _异步操作语言支持提供用于构建分布式系统的语法。_
    

## 能做什么

windows桌面应用程序

Web程序

工控系统

C#最主要的优势，还是用于游戏开发。（查到Unity官方建议C#）

## 如何学习

### [微软官方Docs和在线练习](https://docs.microsoft.com/zh-cn/dotnet/csharp)

过一遍在线教程熟悉一下

#### 声明和使用变量

```c#
string aName = "Bill";
Console.WriteLine(aName);
Console.WriteLine(aName+" is me");
Console.WriteLine($"My name is {aName}");//很熟悉的字符串内插
/*output:
Bill
Bill is me
My name is Bill
*/
```

#### 使用字符串

```c#
string firstFriend = "Maria";
int Lenth = firstFriend.Length;
string secondFriend = "Sage";
Console.WriteLine($"My friends are {firstFriend} and {secondFriend}");
Console.WriteLine($"The name {firstFriend} has {Lenth} letters.");
/*My friends are Maria and Sage
The name Maria has 5 letters.*/
```

#### 发掘字符串的更多精彩用途

```c#
string greeting = "      Hello World!       ";
Console.WriteLine($"[{greeting}]");
// .TrimStart() 去行首空格
string trimmedGreeting = greeting.TrimStart();
Console.WriteLine($"[{trimmedGreeting}]");
// .TrimEnd() 去行尾空格
trimmedGreeting = greeting.TrimEnd();
Console.WriteLine($"[{trimmedGreeting}]");
// .Trim() == .TrimStart().TrimEnd()
trimmedGreeting = greeting.Trim();
Console.WriteLine($"[{trimmedGreeting}]");
// test
string aString = "   ac   ";
Console.WriteLine(aString.TrimStart().TrimEnd());
Console.WriteLine(aString.Trim());
/*
ac
ac
*/
```

#### 发掘字符串的更多精彩用途

```C#
string greeting = "Hello World!       ";
greeting = greeting.Trim();
greeting = greeting.Replace("Hello","Hi");
Console.WriteLine($"{greeting}");
Console.WriteLine($"{greeting.Remove(2)}");
Console.WriteLine($"{greeting.ToUpper()}");
Console.WriteLine($"{greeting.ToLower()}");
Console.WriteLine($"{greeting.ToList()}");
/*
Hi World!
Hi
HI WORLD!
hi world!
System.Collections.Generic.List`1[System.Char]
*/
```

#### 搜索字符串

```c#
string songLyrics = "You say goodbye, and I say hello";
Console.WriteLine(songLyrics.Contains("goodbye"));
var _bool = songLyrics.Contains("goodbye");
Console.Write($"{_bool}\n\r");
Console.WriteLine(songLyrics.Contains("greetings"));
/*True
True

False 少自己加换行符 用WriteLine*/
//try
_bool = songLyrics.StartsWith("You");
Console.Write($"{_bool}\n\r");
//try2
_bool = songLyrics.StartsWith("ou");
Console.Write($"{_bool}\n\r");
//try3
_bool = songLyrics.EndsWith("o");
Console.Write($"{_bool}\n\r");
/*True

False

True*/
```
