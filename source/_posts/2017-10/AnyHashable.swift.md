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
// 在被桥接到 Objective-C 后，取回自定义任何可哈希的值的表示
@_inlineable // FIXME(sil-serialize-all)
@_versioned // FIXME(sil-serialize-all)
internal func _getBridgedCustomAnyHashable<T>(_ value: T) -> AnyHashable? {
  let bridgedValue = _bridgeAnythingToObjectiveC(value)
  return (bridgedValue as? _HasCustomAnyHashableRepresentation)?._toCustomAnyHashable()
}
#endif
```