---
title: Swift Standard Library（Swift 标准库）
date: {{ date }}
categories: Apple文档
tags: apple docs
comment: true
---
[原文地址](https://developer.apple.com/documentation/swift)

解决复杂问题和写高性能，易读的代码。

## 概览（Overview）

Swift 标准库为写 Swift 程序定义了内层功能，包括：

- 基本数据类型，例如 Int，Double 和 String
- 常见数据结构，例如数组（Array），字典（Dictionary），和集合（Set）
- 全局函数，例如 `print(_:separator:teminator:)` 和 `abs(_:)`
- 描述常见抽象的协议，例如 Collection 和 Equatable

> ###### 体验标准库
>
> 体验 Swift 标准库类型和使用可视化和实际的例子学习高级概念。学习 Swift 标准库如何使用协议（protocol）和泛型（generics）表达强大的约束。下载下面的 playground 来开始。
>
> [Swift Standard Library.plyground](https://developer.apple.com/sample-code/swift/downloads/standard-library.zip)

## 主题（Topics）

### 值和集合（Values and Collections）

##### [数值和基础值（Numbers and Basic Values）](/2017/09/24/2017-09/Numbers-and-Basic-Values）/)

用数值，布尔值和其他基础类型模型数据。

##### [字符串和文本（Strings and Text）](https://developer.apple.com/documentation/swift/strings_and_text)

使用 Unicode-safe 字符串文本工作。

##### [集合（Collections）](https://developer.apple.com/documentation/swift/collections)

使用数组，字典，集合和其他数据结构存储和组织数据。

### 用于你的类型的工具（Tools for your Types）

##### [基本行为（Basic Behaviors）](https://developer.apple.com/documentation/swift/basic_behaviors)

取决于相等性或顺序的测试和作为集合和字典的成员来在工作中使用你的自定义类型。

##### [编码，解码和序列化（Encoding，Decoding，and Serialization）](https://developer.apple.com/documentation/swift/encoding_decoding_and_serialization)

用隐式或自定义编码序列化和反序列化你的类型的实例。

##### [用字面量初始化（Initialization with Literals）](https://developer.apple.com/documentation/swift/initialization_with_literals)

使用不懂种类的字面量允许你的类型的值被表达。

### 编程任务（Programming Tasks）

##### [输入和输出（Input and Output）](https://developer.apple.com/documentation/swift/input_and_output)

打印到控制台，从文本流读取和写入到文本流，以及使用命令行参数。

##### [调试和反射（Debugging and Reflection）](https://developer.apple.com/documentation/swift/debugging_and_reflection)

使用运行时检查增强你的代码并检查你的值得运行时描述。

##### [关键路径表达式（Key-Path Expressions）](https://developer.apple.com/documentation/swift/key_path_expressions)

使用关键路径表达式动态地访问属性。

##### [手动内存管理（Manual Memory Management）](https://developer.apple.com/documentation/swift/manual_memory_management)

手动分配和管理内存。

##### [类型转换和存在类型（Type Casting and Existential Types）](https://developer.apple.com/documentation/swift/type_casting_and_existential_types)

在类型或任何类型的代表值之间执行转换。

##### [C 互操作性（C Interoperability）](https://developer.apple.com/documentation/swift/c_interoperability)

使用导入的 C 类型或调用 C 的可变（variadic）函数。

##### [操作符声明（Operator Declarations）](https://developer.apple.com/documentation/swift/operator_declarations)

使用前缀，后缀和中缀操作符。