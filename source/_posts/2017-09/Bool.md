---
title: Bool（布尔型）
date: {{ date }}
categories: Apple文档
tags: Numbers and Basic Values
comment: true
---
###### Structure

实例是真或假的值类型。

## 概览（Overview）

在 Swift 里 Bool 代表布尔值。通过使用布尔值真（true）或假（false）中的一个，或者通过赋值布尔方法或操作的结果到变量或常量来创建 Bool 实例。

```swift
var godotHasArrived = false

let numbers = 1...5
let containsTen = numbers.contains(10)
print(containsTen)
// Prints "false"

let (a, b) = (100, 101)
let aFirst = a < b
print(aFirst)
// Prints "true"
```

在条件比较的环境里 Swift 只是用简单的布尔值帮助避免意外的编程错误和帮助维持每个控制语句的清晰。不像在其他编程语言里一样，在 Swift 里，整型和字符串不能被使用在要求是布尔值的地方。

例如，以下代码示例不会编译，因为他尝试在逻辑环境下使用整型 `i`。

```swift
var i = 5
while i {
  print(i)
  i -= 1
}
```

在 Swift 里正确的方法是在 while 语句中比较 i 和 0。

```swift
while i != 0 {
  print(i)
  i -= 1
}
```

### 使用导入的布尔值（Using Imported Boolean values）

C bool 和 Boolean 类型和 Objective-C BOOL 类型都被桥接到 Swift 里作为 Bool。在 Swift 里单一的 Bool 类型保证了从 C 和 Objective-C 里导入的函数（functions），方法（methods）和属性（properties）有一个一致的类型接口。

## 主题（Topics）

### 比较布尔值（Comparing Boolean Values）

`static func ==(Bool)`

`static func !=(Bool, Bool)`

返回表示两个值是否不相等的布尔值。‘

### 转换一个布尔值（Transforming a Boolean）

`static func !(Bool)`

在一个布尔值上执行逻辑非操作。

`static func ||(Bool, () -> Bool)`

在两个布尔值上执行逻辑或操作。

`static func &&(Bool, () -> Bool)`

在两个布尔值上执行逻辑与操作。

### 描述一个布尔值（Describing a Boolean）

[`var` description`: String`](https://developer.apple.com/documentation/swift/bool/1538894-description)

布尔值的文本表示。

### 检查布尔值（Inspecting a Boolean）

[`var` customMirror`: Mirror`](https://developer.apple.com/documentation/swift/bool/1641275-custommirror)

反射布尔值的一个镜像。

[`var` customPlaygroundQuickLook`: PlaygroundQuickLook`](https://developer.apple.com/documentation/swift/bool/1641275-custommirror)

[`var` hashValue`: Int`](https://developer.apple.com/documentation/swift/bool/1540169-hashvalue)

布尔值哈希值。

### 从另一个值创建布尔值（Creating a Boolean From Another Value）

[init`(Bool)`](https://developer.apple.com/documentation/swift/bool/1540923-init)

创建一个等于给定布尔值的实例。

[init`?(String)`](https://developer.apple.com/documentation/swift/bool/2428231-init)

从给定字符串创建一个新的布尔值。

[init(truncating: `NSNumber`)](https://developer.apple.com/documentation/swift/bool/2895323-init)

[init?(exactly: `NSNumber`)](https://developer.apple.com/documentation/swift/bool/2895341-init)

### 编码和解码（Encoding and Decodiing）

[init(from: `Decoder`)](https://developer.apple.com/documentation/swift/bool/2894515-init)

[`func` encode(to: `Encoder`)](https://developer.apple.com/documentation/swift/bool/2894902-encode)

### 极少使用的初始化器（Infrequently Used Initializers）

[init()](https://developer.apple.com/documentation/swift/bool/1539399-init)

创建一个初始化为假的实例。

[init(booleanLiteral: `Bool`)](https://developer.apple.com/documentation/swift/bool/1540965-init)

创建一个初始化为指定布尔值字面量的实例。

## 关系（Relationships）

### 遵循（协议）（Conforms To）

- [CustomPlaygroundQuickLookable](https://developer.apple.com/documentation/swift/customplaygroundquicklookable)
- [CustomReflectable](https://developer.apple.com/documentation/swift/customreflectable)
- [CustomStringConvertible](https://developer.apple.com/documentation/swift/customstringconvertible)
- [Equatable](https://developer.apple.com/documentation/swift/equatable)
- [ExpressibleByBooleanLiteral](https://developer.apple.com/documentation/swift/expressiblebybooleanliteral)
- [Hashable](https://developer.apple.com/documentation/swift/hashable)
- [LosslessStringConvertible](https://developer.apple.com/documentation/swift/losslessstringconvertible)

