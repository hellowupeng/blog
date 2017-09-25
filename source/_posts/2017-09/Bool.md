# Bool（布尔型）

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

## 话题（Topics）

## 关系（Relationships）