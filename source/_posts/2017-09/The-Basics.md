---
title: The Basics
date: {{ date }}
categories: Swift
tags: iOS
comment: true
---
Swift 是一种用于iOS，macOS，watchOS 和 tvOS 应用开发的新语言。尽管如此，Swift 的许多部分会和你使用 C 和 Objective-C 的开发体验面熟。

Swift 为所有基础的 C 和 Objective-C 类型提供了自己的版本，包括对于整型的 `Int`，对于浮点值的 `Double` 和 `Float`，对于布尔值的 `Bool` 和 对于文本数据的 `String`。Swift 也提供了三种主要集合类型，`Array` 、 `Set` 和 `Dictionary`，在集合类型（Collection Types）里有描述。

像 C 一样，Swift 使用变量来存储并通过一个标识名来引用值。Swift 也广泛使用值不能改变的变量。也就是大家所熟知的常量，比 C 里面的常量更加强大。在整个 Swift 中，当你使用不需要改变的值的时候，常量被用来让代码更加安全和清晰。

除了熟悉的类型以外，Swift 还介绍了在 Objective-C 里面没有的高级类型，例如元组（tuples）。元组使你能够创建并传递一组值。你可以使用元组作为一个单一的复合值从一个函数返回多个值。

Swift 也介绍了处理值缺失的可选型。可选型表示要么“这里有值，它等于x”，要么“这里根本没有值”。使用可选型类似于在 Objective-C 里对于指针使用 `nil` ，但是它们适用于任何类型，不仅仅是类。可选型不仅比 Objective-C 里的 nil 指针更安全和更具表达性，而且它们还是 Swift 的许多强大特性的核心。

Swift 是一种类型安全的语言，这意味着语言帮助你让你的代码的值类型清晰。如果你的部分代码需要一个 `String` ，类型安全阻止你错误地传入一个 `Int` 。同样地，类型安全阻止你意外地传入一个可选型（optional）`String` 到需要非可选型（nonoptional）`String` 的一段代码。类型安全帮助你在开发过程中尽可能早地捕获并解决错误。

## 常量和变量（Constants and Variable）

常量和变量把一个名称（例如 `maximumNumberOfLoginAttempts` 或 `welcomeMessage`）和一个特定类型的值（例如数字 `10` 或字符串 `"Hello"` ）联系起来。常量的值一旦设置就不能改变，而变量在将来可以被设置不同的值。

### 声明常量和变量（Declaring Constants and Variables）

### 类型注解（Type Annotation）

### 命名常量和变量（Naming Constants and Variables）

### 打印常量和变量（Printing Constants and Variables）

## 注释（Comments）

## 分号（Semicolons）

## 整型（Integers）

### 整型边界（Integer ）

### Int

### UInt

## 浮点值（Floating-Point Numbers）

## 类型安全和类型引用（Type Safety and Type Inference）

## 数值字面量（Numeric Literals）

## 数值类型转换（Numeric Type Conversion）

### 整型转换（Integer Conversion）

### 整型和浮点型转换（Integer and Floating-Point Conversion）

## 类型别名（Type Aliases）

## 布尔型（Booleans）

## 元组（Tuples）

## 可选型（Optionals）

## nil

## If 语句和强制解包（If Statements and Forced Unwrapping）

## 可选绑定（Optional Binding）

## 隐式可选型解包（Implicitly Unwrapped Optional）

## 错误处理（Error Handling）



