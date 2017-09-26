# CustomStringConvertible

###### Protocol

用自定义文本表示的类型。

## 概览（Overview）

遵循 `CustomStringConvertible` 协议的类型当转变实例为字符串时，能提供他们自己的表示来使用。要转换任何类型的实例到字符串，`String(describing:)` 初始化器是较好的方式。如果传入的实例遵循 CustomStringConvertible，`String(describing:)` 初始化器和 `print(_:)` 函数使用实例的自定义 `description` 属性。

直接访问一个类型的 `description` 属性或使用 `CustomStringConvertible` 作为通用约束是被阻止的。 

### 遵循 CustomStringConvertible 协议

通过定义一个 `description` 属性添加遵循的 CustomStringConvertible 到你的自定义类型。

例如，这个自定义 Point 结构使用由标准库提供的默认表示。

```swift
struct Point {
    let x: Int, y: Int
}

let p = Point(x: 21, y: 30)
print(p)
// Prints "Point(x: 21, y:30)"
```

在实现 `description` 属性并声明遵循的 CustomStringConvertible 后，Point 类型提供它自己的自定义表示。

```swift
extension Point: CustomStringConvertible {
    var description: String {
        return "(\(x), \(y))"
    }
}

print(p)
// Prints "(21, 30)"
```



## 主题（Topics）

### 实例属性（Instance Properties）

###### var description: String

实例的文本表示。

##### 必要的

## 关系（Relationships）

### 继承的类（Inherit By）

###### BinaryInteger, LosslessStringConvertible, ReferenceConvertible

### 采用的类（Adopted By）

###### AffineTransform，IndexSet

###### AnyHashable，IndexSet.Index

###### Array，Locale

###### ArraySlice，MeasureMent

###### Bool， Mirror

Calendar，NSObject

CGFloat，ObjcBool

Character，PersonNameComponent

CharacterSet，Range，

ClosedRange，Selector

ContiguousArray，Set

CountableClosedRange，StaticString

CountableRange，String

Data，String.Unicode

Date，ScalarView

DateComponents，String.UTF16View

DateInterval，String.UTF8View

Decimal，Substring

Dictionary，TimeZone

Double，Unicode.Scalar

Float，URL

Float80，URLComponents

ImplicitlyUnwrappedOptional，URLQueryItem

IndexPath，URLRequest，UUID

## 另见（See Also）

### 字符串表示（String Representation）

###### protocol LosslessStringConvertible

能被用无损的，清晰地方式表示为字符串的类型。

###### protocol CustomDebugStringConvertible

适用于调试目的自定义的文本表示的类型。