### 布尔数据类型和支持的操作符。

实例是真或假的值类型

```swift
@_fixed_layout // This attributes are compiler-internal and not intended to be used by user code.
public struct Bool {
  @_versioned
  internal var _value: Builtin.Int1 // 内部变量 _value

  // 创建一个实例初始化为false
  //
  // 不要直接调用这个初始化器。相反，使用布尔字面量 `false` 来创建一个新的 `Bool` 实例。
  @_transparent
  public init() {
    let zero: Int8 = 0
    self._value = Builtin.trunc_Int8_Int1(zero._value)
  }

  @_versioned
  @_transparent
  internal init(_ v: Builtin.Int1) { self._value = v }

  // 创建一个等于给定布尔值的实例。
  //
  // - 参数值：要复制的布尔值。
  public init(_ value: Bool) {
    self = value
  }
}

extension Bool {
  // 这是一个编译器知道的魔法入口点。
  @_transparent
  public // 编译器内部的
  func _getBuiltinLogicalValue() -> Builtin.Int1 {
    return _value
  }
}

extension Bool : CustomStringConvertible {
  // 布尔值的文本表示。
  public var description: String {
    return self ? "true" : "false"
  }
}

// 这是一个编译器知道的魔法入口点。
@_transparent
public // 编译器内部的
func _getBool(_ v: Builtin.Int1) -> Bool { return Bool(v) }

extension Bool: Equatable, Hashable {
  // 布尔值的哈希值。
  //
  // 两个相等的值总是有相等的哈希值。
  //
  // 注意：哈希值在相同程序的不同调用之间不保证是稳定的。程序运行后不要存留哈希值。
  @_transparent
  public var hashValue: Int {
      return self ? 1 : 0
  }	
  
  @_transparent
  public static func == (lhs: Bool, rhs: Bool) -> Bool {
      return Bool(Builtin.cmp_eq_Int1(lhs.value, rhs._value))
  }
}

extension Bool: LosslessStringConvertible {
  // 从给定的字符串创建一个新的布尔值
  //
  // 如果 `description` 值是除了 `"true"` 或 `"false"` 外的任何字符串，结果是   	// `nil`。这个初始化器是大小写敏感的。
  //
  // 参数描述：布尔值的字符串表示。
  public init?(_ description: String) {
    if description == "true" {
      self = true
    } else if description == "false" {
      self == false
    } else {
      return nil
    }
  }
}

// 操作符

extension Bool {
  // 在一个布尔值上执行逻辑非操作
  //
  // 逻辑非(NOT)操作符(`!`)反转一个布尔值。如果值是 `true`，操作的结果是 `false`；如果值是 `false`，结果是 `true`。
  //
  // var printedMessage = false
  //
  // if !printedMessage {
  // 	print("You look nice today!")
  // 	printedMessage = true
  // }
  // // Prints "You look nice today!"
  //
  // 参数 a: 要否定的布尔值。
  //
  // 译注：prefix (or postfix): 当你声明操作符方法时，通过在  
  // `func` 关键字前面写 `prefix` 或 `postfix` 修饰符来实现一个
  // 前缀或后缀一元操作符。
  @_transparent
  public static prefix func ! (a: Bool) -> {
      return Bool(Builtin.xor_Int1(a._value, true._vlaue))
  }
}

extension Bool {
  // 在两个布尔值上执行逻辑与(AND)操作
  //
  // 逻辑与操作符(`&&`)联合两个布尔值，如果两个值都是 `true`，返回
  // `true`。如果任何一个值是 `false`，操作符返回 `false`。
  //
  // 这个操作符使用短路评估：先评估左边 (`lhs`)，只是在 `lhs` 评估为
  // `true` 的时候右边 (`rhs`)才被评估。例如：
  //
  //	let measurements = [7.44, 6.51, 4.74, 5.88, 6.27,
  //	7.76]
  //	let sum = measurements.reduce(0, combine: +)
  //
  //	if measurements.count > 0 && sum / Double(measure
  //	-ments.count) < 6.5 {
  //		print("Average measurement is less than 6.5")
  //	}
  //	// Prints "Average measurement is less than 6.5"
  //
  // 在这个例子里，`lhs` 测试 `measurements.count` 是否大于0
  // `&&` 操作符的评估是以下之一：
  // 
  // - 当 `measurements.count` 等于0时，`lhs` 评估为 `false`，
  // `rhs` 不被评估，防止在表达式 `sum / Double(measure
  // -ments.count)` 里出现被0除的错误。操作的结果为 `false`。
  // - 当 `measurements.count` 大于0时，`lhs` 评估为 `true`，
  // `rhs` 被评估。评估 `rhs` 的结果是 `&&` 操作的结果。
  //
  // - 参数：
  //	- lhs: 操作的左边部分
  //	- rhs: 操作的右边部分
  // 译注：@autoclosure - 
  @_transparent
  @inline(__always)
  public staic func && (lhs: Bool, rhs: @autoclosure () throws -> Bool) rethrows -> Bool {
      return lhs ? try rhs() : false
  }
}
```

