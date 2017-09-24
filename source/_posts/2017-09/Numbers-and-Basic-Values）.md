# 数值和基本值（Numbers and Basic Values）

用数值，布尔值和其他基础类型模型化数据。

## 主题（Topics）

### 逻辑值（Logical Values）

`struct` [Bool](https://developer.apple.com/documentation/swift/bool)

实例是真（true）或假（false）的值类型。

### 数值（Numeric Values）

`struct` [Int](https://developer.apple.com/documentation/swift/int)

有符号整数类型。

`struct` [Double](https://developer.apple.com/documentation/swift/double)

双精度，浮点值类型。

`struct` [Float](https://developer.apple.com/documentation/swift/float)

单精度，浮点值类型。

### 范围（Ranges）

`struct` [Range](https://developer.apple.com/documentation/swift/range)

在一种可比较类型之上的半开区间，从下界到上界，但是不包括下界。

`struct` [ClosedRange](https://developer.apple.com/documentation/swift/closedrange)

在一种可比较类型之上的闭区间，从下界到上界，包括下界。

### 错误（Errors）

`protocol` [Error](https://developer.apple.com/documentation/swift/error)

代表一种能被抛出的错误值。

### 可选型（Optionals）

`enum` [Optional](https://developer.apple.com/documentation/swift/optional)

代表是一种包裹的值（wrapped）或空（nil）—值的缺失的类型。

### 高级数值（Advanced Numerics）

##### [数值协议（Numeric Protocols）](https://developer.apple.com/documentation/swift/numbers_and_basic_values/numeric_protocols)

写和任何数值类型工作的通用代码。

##### [特殊使用的数值类型（Special-Use Numeric Types）](https://developer.apple.com/documentation/swift/numbers_and_basic_values/special_use_numeric_types)

和不同尺寸的固定宽度数值类型工作。

##### [全局数值函数（Global Numeric Function）](https://developer.apple.com/documentation/swift/numbers_and_basic_values/global_numeric_functions)

与数值和其他可比较类型使用这些函数。

## 另见（See Also）

### 值和集合（Values and Collections）

##### [字符串和文本（Strings and Text）](https://developer.apple.com/documentation/swift/strings_and_text)

和使用 Unicode-safe 的字符串的文本工作。

##### [集合（Collections）](https://developer.apple.com/documentation/swift/collections)

使用数组，字典，集合和其他数据结构存储和组织数据。