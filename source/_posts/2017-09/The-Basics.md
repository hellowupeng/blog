---
title: The Basics（基础）
date: {{ date }}
categories: Swift
tags: swift programming guide
comment: true
---
[原文地址](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/TheBasics.html#//apple_ref/doc/uid/TP40014097-CH5-ID309)

Swift 是一种用于iOS，macOS，watchOS 和 tvOS 应用开发的新语言。尽管如此，Swift 的许多部分会和你使用 C 和 Objective-C 的开发体验面熟。

Swift 为所有基础的 C 和 Objective-C 类型提供了自己的版本，包括对于整型的 `Int`，对于浮点值的 `Double` 和 `Float`，对于布尔值的 `Bool` 和 对于文本数据的 `String`。Swift 也提供了三种主要集合类型，`Array` 、 `Set` 和 `Dictionary`，在集合类型（Collection Types）里有描述。

像 C 一样，Swift 使用变量来存储并通过一个标识名来引用值。Swift 也广泛使用值不能改变的变量。也就是大家所熟知的常量，比 C 里面的常量更加强大。在整个 Swift 中，当你使用不需要改变的值的时候，常量被用来让代码更加安全和清晰。

除了熟悉的类型以外，Swift 还介绍了在 Objective-C 里面没有的高级类型，例如元组（tuples）。元组使你能够创建并传递一组值。你可以使用元组作为一个单一的复合值从一个函数返回多个值。

Swift 也介绍了处理值缺失的可选型。可选型表示要么“这里有值，它等于x”，要么“这里根本没有值”。使用可选型类似于在 Objective-C 里对于指针使用 `nil` ，但是它们适用于任何类型，不仅仅是类。可选型不仅比 Objective-C 里的 nil 指针更安全和更具表达性，而且它们还是 Swift 的许多强大特性的核心。

Swift 是一种类型安全的语言，这意味着语言帮助你让你的代码的值类型清晰。如果你的部分代码需要一个 `String` ，类型安全阻止你错误地传入一个 `Int` 。同样地，类型安全阻止你意外地传入一个可选型（optional）`String` 到需要非可选型（nonoptional）`String` 的一段代码。类型安全帮助你在开发过程中尽可能早地捕获并解决错误。

## 常量和变量（Constants and Variable）

常量和变量把一个名称（例如 `maximumNumberOfLoginAttempts` 或 `welcomeMessage`）和一个特定类型的值（例如数字 `10` 或字符串 `"Hello"` ）联系起来。常量的值一旦设置就不能改变，而变量在将来可以被设置不同的值。

### 声明常量和变量（Declaring Constants and Variables）

常量和变量在使用前必须先声明。你使用 `let` 关键字声明常量，`var` 关键字声明变量。这里有一个常量和变量怎样被用来追踪用户做出的登录尝试次数的例子。

```swift
let maximumNumberOfLoginAttempts = 10
var currentLoginAttempt = 0
```

这段代码可以被理解为：

“声明一个叫做 `maximumNumberOfLoginAttempts` 的常量，并赋值10。然后，声明一个叫做 `currentLoginAttempt` 的变量，并赋初始值0。”

在这个例子里，允许登录尝试的最大次数被声明为一个常量，因为最大次数从不改变。当前登录尝试计数器被声明为一个变量，因为这个值在每次登录尝试失败后必须被增加。

你可以在单行里声明多个常量或多个变量，由逗号分隔。

```swift
var x = 0.0, y = 0.0, z = 0.0
```

> ###### 注意
>
> 如果在你的代码里一个存储值不会改变，总是使用 `let` 关键字声明它为常量。只对于需要能够被改变的存储值使用变量。

### 类型注解（Type Annotation）

当你声明一个常量或变量的时候，你可以提供一个类型注解（type annotation），使常量和变量能存储的值的种类变得清晰。通过在常量或变量名后放置一个分号，跟着一个空格，跟着要使用的类型名来写类型注解。

这个例子为一个叫做 `welcomeMessage` 的变量提供一个类型注解，表示这个变量能存储 `String` 值：

```swift
var welcomeMessage: String
```

这个声明里的分号意思是“…类型的…”，所以上面的代码可理解为：

“声明一个叫做 `welcomeMessage` 叫做 `String` 类型的变量。”

短语“ `String` 类型的（of type `String`）”意思是“能存储任何 `String` 值。”把它看做意思是能被存储的“东西的类型”（或“东西的种类”）。

`welcomeMessage` 变量现在能被设置为任何字符串而不会报错。

```swift
welcomeMessage = "Hello"
```

你可以在一行上定义相同类型的多个相关的变量，由逗号分隔，在最后的变量名后加上一个类型注解。

```swift
var red, green, blue: Double
```

> ###### 注意
>
> 在实践中你很少需要写类型注解。如果你为常量或变量在定义时提供一个初始值，Swift 几乎总是能推断出常量或变量要使用的类型，在[类型安全和类型推导（Type Safety and Type Inference）]()里有描述。在上面的 `welcomeMessage` 例子里，没有提供初始值，所以 `welcomeMessage` 变量的类型使用类型注解指定而不是从初始值推断。

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

## 断言和前提条件（Assertions and Preconditions）

## 使用断言调试（Debugging with Assertions）

## 强制前提条件（Enforcing Preconditions）