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
```

