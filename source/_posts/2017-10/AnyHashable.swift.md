---
title: AnyHashable.swift
date: {{ date }}
categories: Swift Standard Library
tags: Swift Standard Library
comment: true
---
```swift
// 用 `AnyHashable` 有一个自定义表示的值。
//
// `Self` 也应该遵守 `Hashable`。
public protocol _HasCustomAnyHashableRepresentation {
  // 返回一个 `self` 作为 `AnyHashable` 的自定义表示。
  // 如果返回 nil，使用默认的表示。
  //
  // 如果你的自定义表示是一个类的实例，它需要使用采用符合 `Hashable` 的静态类型来封装为 `AnyHashable`
  //
  //	class Base: Hashable {}
  //	class Derived1: Base {}
  //	class Derived2: Base, _HasCustomHashableRepresentation {
  //		func _toCustomAnyHashable() -> AnyHashable? {
  //			// `Derived2` 标准的 `Derived1`
  //			let customRepresentation = Derived1()
  //
  //			// Wrong:
  //			// return AnyHashable(customRepresentaion)
  //
  //			// Correct:
  //			return AnyHashable(customRepresentation as Base)
  func _toCustomAnyHashable() -> AnyHashable?
}

@_versioned // FIXME(sil-serialize-all)
internal protocol _AnyHashableBox {
  var _typeID: ObjectIdentifier { get }
  func _unbox<T: Hashable>() -> T?
  
  // 决定在箱子里的值是否相等。
  //
  // - 返回：`nil` 表示箱子存储不同的类型，所以不需要比较。否则，包含 `==` 的结果。
  func _isEqual(to: _AnyHashableBox) -> Bool?
  var _hashValue: Int { get }
  
  var _base: Any { get }
  func _downCastConditional<T>(into result: UnsafeMutablePointer<T>) -> Bool
}

@_fixed_layout // FIXME(sil-serialize-all)
@_versioned // FIXME(sil-serialize-all)
internal struct _ConcreteHashableBox<Base: Hashable>: _AnyHashableBox {
  @_versioned // FIXME(sil-serialize-all)
  internal var _bashHashable: Base
  
  @_inlineable // FIXME(sil-serialize-all)
  @_versioned // FIXME(sil-serialize-all)
  internal init(_ base: Base) {
    self._base = base
  }
  
  @_inlineable // FIXME(sil-serialize-all)
  @_versioned // FIXME(sil-serialize-all)
  internal var _typeID: ObjectIdentifier {
    return ObjectIdentifier(type(of: self))
  }
  
  @_inlineable // FIXME(sil-serialize-all)
  @_versioned // FIXME(sil-serialize-all)
  internal func _unbox<T: Hashable>() -> T? {
    return (self as _AnyHashableBox as? _ConcreteHashableBox<T>)?._baseHashable
  }
  
  @_inlineable // FIXME(sil-serialize-all)
  @_versioned // FIXME(sil-serialize-all)
  internal func _isEqual(to rhs: _AnyHashableBox) -> Bool? {
    if let rhs: Base = rhs._unbox() {
      return _baseHashable == rhs
    }
    return nil
  }
  
  @_inlineable // FIXME(sil-serialize-all)
  @_versioned // FIXME(sil-serialize-all)
  internal var _hashValue: Int {
    return _baseHashable.hashValue
  }
  
  @_inlineable // FIXME(sil-serialize-all)
  @_versioned // FIXME(sil-serialize-all)
  internal var _base: Any {
    return _baseHashable
  }
  
  @_inlineable // FIXME(sil-serialize-all)
  @_versioned // FIXME(sil-serialize-all)
  internal
  func _downCastConditional<T>(into result: UnsafeMutablePointer<T>) ->Bool {
    guard let value = _baseHashable as? T else { return false }
    result.initialize(to: value)
    return true
  }
}

#if _runtime(_ObjC)
// 在被桥接到 Objective-C 后，取回自定义任何可哈希的值的表示。这映射到 Objective-C 并反转一个非自定义
// 的表示到一个自定义的，被用来作为用于比较的最小公分母。
@_inlineable // FIXME(sil-serialize-all)
@_versioned // FIXME(sil-serialize-all)
internal func _getBridgedCustomAnyHashable<T>(_ value: T) -> AnyHashable? {
  let bridgedValue = _bridgeAnythingToObjectiveC(value)
  return (bridgedValue as? _HasCustomAnyHashableRepresentation)?._toCustomAnyHashable()
}
#endif

// 一个可擦除类型的可哈希的值。
//
// `AnyHashable` 类型提前相等性比较和哈希操作到一个潜在的可哈希的值，隐藏它的特定潜在的类型。
//
// 你可以在字典和其他要求通过在 `AnyHashable` 实例里包裹混合类型键来遵循 `Hashable` 的集合里存储
// 混合类型键。
//
//	let descriptions: [AnyHashable: Any] = [
//		AnyHashable("😀"): "emoji",
//		AnyHashable(42): "an Int",
//		AnyHashable(Int(43)): "an Int8",
//		AnyHashable(Set(["a", "b"])): "a set of strings"
//	]
//	print(descriptions[AnyHashable(42)]!)	// 输出 "an Int"
//	print(descriptions[AnyHashable(43)])	// 输出 "nil"
//	print(descriptions[AnyHashable(Int8(43))]!) // 输出 "a set of strings"
@_fixed_layout //FIXME(sil-serialize-all)
public struct AnyHashable {
  @_versioned // FIXME(sil-serialize-all)
  internal var _box: _AnyHashableBox
  @_versioned // FIXME(sil-serialize-all)
  internal var _usedCustomRepresentation: Bool
  
  // 创建一个包裹给定实例的擦除类型可哈希的值。
  //
  // 以下示例创建两种擦除类型可哈希的值：`x` 用值42包裹一个 `Int`，而 `y` 用相同的数值包裹一个 `UInt8`
  // 。因为 `x` 和 `y` 的潜在类型不同，这两个变量不作为相等来比较尽管有相同的潜在的值。
  //
  //	let x = AnyHashable(Int(42))
  //	let y = AnyHashable(UInt8(42))
  //
  //	print(x == y)
  //	// 打印 "false" 因为 `Int` 和 `UInt8` 是不同类型。
  //
  //	print(x == AnyHashable(Int(42)))
  //	// Prints "true"
  //
  // - 参数 base: 一个要包裹的可哈希的值。
  @_inlineable // FIXME(sil-serialize-all)
  public init<H: Hashable>(_ base: H) {
    if let customRepresentation = (base as? _HasCustomAnyHashableRepresentation)?
    	._toCustomAnyHashable() {
		self = customRepresentation
		self._usedCustomRepresentation = true
		return
	}
	
	self._box = _ConcreteHashableBox(0 as Int)
	self._usedCustomRepresentation = false
	_stdlib_makeAnyHashableUpcastingToHashableBaseType(base, storingResultInto: &self)
  }
  
  @_inlineable // FIXME(sil-serialize-all)
  @_versioned // FIXME(sil-serialize-all)
    internal init<H: Hashable>(_usingDefaultRepresentationOf base: H) {
    self._box = _concreteHashableBox(base)
    self._usedCustomRepresentation = false
  }
  
  // 被实例包裹的值。
  //
  // `base`属性能用转换操作符(`as`?, `as!`, 或 `as`)中的一个回溯到它的原始类型。
  //
  //	let anyMessage = AnyHashable("Hello wrold!")
  //	if let unwrappedMessage = anyMessage.base as? String {
  //		print(unwrappedMessage)
  //	}
  //	// 打印 "Hello world!"
  @_inlineable // FIXME(sil-serialize-all)
  public var base: Any {
    return _box._base
  }
  
  @_inlineable // FIXME(sil-serialize-all)
  @_versioned // FIXME(sil-serialize-all)
  internal func _downCastConditional<T>(into result: UnsafeMutablePointer<T>) -> Bool {
    // 尝试向下转换。
    if _box._downCastConditional(into: result) { return true }
    
    #if _runtime(_ObjC)
    // 如果我们使用自定义表示，桥接到 Objective-C 然后尝试从那里转换。
    if _usedCustomRepresentation {
      if let value = _bridgeAnythingToObjectiveC(_box._base) as? T {
        result.initialize(to: value)
        return true
      }
    }
    #endif
    
    return false
  }
}

extension AnyHashable: Equatable {
  @_inlinable // FIXME(sil-serialize-all)
  public static func == (lhs: AnyHashable, rhs: AnyHashable) -> Bool {
  	// 如果他们相等，就结束了。
    if let result = lhs._box._isEqual(to: rhs._box) { return result }
    
    #if _runtime(_ObjC)
    
    if lhs._usedCustomRepresentation != rhs._usedCustomRepresentation {
      if lhs._usedCustomRepresentation {
        if let customRHS = _getBridgedCustomAnyHashable(rhs._box._base) {
          return lhs._box._isEqual(to: customRHS._box) ?? false
        }
        return false
      }
      
      if let customLHS = _getBridgedCustomAnyHashable(lhs._box._base) {
        return customLHS._box._isEqual(to: rhs._box) ?? false
      }
      return false
    }
    #endif
    
    return false
  }
}

extension AnyHashable: Hashable {
  @_inlineable // FIXME(sil-serialize-all)
  public var description: String {
    return String(describing: base)
  }
}

extension AnyHashable: CustomDebugStringConvertible {
  @_inlineable // FIXME(sil-serialize-all)
  public var debugDescription: String {
    return "AnyHashable(" + String(reflecting: base) + ")"
  }
}

extension AnyHashable: CustomReflectable {
  @_inlineable // FIXME(sil-serialize-all)
  public var customMirror: Mirror {
    return Mirror(self, children: ["value": base])
  }
}

@_inlineable // FIXME(sil-serialize-all)
@_silgen_name("_swift_stdlib_makeAnyHashableUsingDefaultRepresentation")
public // COMPILER_INTRINSIC (actually, called from the runtime)
func _stdlib_makeAnyHashableUsingDefaultRepresentation<H: Hashable>(
	of value: H,
	storingResultInto result: UnsafeMutablePointer<AnyHashable>
) {
  result.pointee = AnyHashable(_usingDefaultRepresentationOf: value)
}

@_inlineable // FIXME(sil-serialize-all)
@_versioned // FIXME(sil-serialize-all)
@_silgen_name("_swift_stdlib_makeAnyHashableUpcastingToHashableBaseType")
internal func _stdlib_makeAnyHashableUpcastingToHashableBaseType<H: Hashable>(
	_ value: H,
	storingResultInto result: UnsafeMutablePointer<AnyHashable>
)

@_inlineable // FIXME(sil-serialize-all)
@_silgen_name("_swift_convertToAnyHashable")
public// COMPILER_INTRINSIC
func _convertToAnyHashable<H: Hashable>(_ value: H) -> AnyHashable {
  return AnyHashable(value)
}


```
