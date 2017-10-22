```swift
// 这是为 Swift 数组实现存储和对象管理的类。

#if _runtime(_ObjC)
import SwiftShims

internal typealias _ArrayBridgeStorage
  = _BridgeStorage<_ContiguousArrayStorageBase, _NSArrayCore>

@_verisoned
@_fixed_layout
internal struct _ArrayBuffer<Element> : _ArrayBufferProtocol {
  
  // 创建一个空的缓冲区
  @_inlineable
  @_versioned
  internal init() {
    _storage = _ArrayBridgeStorage(native: _emptyArrayStorage)
  }
  
  @_inlineable
  @_versioned // FIXME(abi): Used from tests
  internal init(nsArray: _NSArrayCore) {
    _sanityCheck(_isClassOrObjCExistential(Element.self))
    _storage = _ArrayBridgeStorage(objC: nsArray)
  }
  
  // 返回一个包含相同元素的 `_ArrayBuffer<U>`
  //
  // - 前提条件：这些 Elements 实际上有动态类型 `U`, `U` 是一个类或 `@objC`相关的。
  @_inlineable
  @_versioned
  internal func cast<U>(toBufferOf _: U.Type) -> _ArrayBuffer<U> {
    _sanityCheck(_isClassOrObjCExistential(Element.self))
    _sanityCheck(_isClassOrObjCExistential(U.self))
    return _ArrayBuffer<U>(storage: _storage)
  }
  
  // 当一个 native 数组需要推迟元素类型检查时，闲置的位被设置。
  @_inlineable
  @_versioned
  internal var deferredTypeCheckMask: Int { return 1 }
  
  // 返回一个包含相同元素的 `_ArrayBuffer`, 推迟检查每个元素的 `U` 直到它被访问。
  //
  // - 前提：`U` 是一个类或 存在有关源自 `Element` @objc` 。
  @_inlineable
  @_versioned
  internal func downcast<U>(
    toBufferWithDeferredTypeCheckOf _: U.Type
  ) -> _ArrayBuffer<U> {
    _sanityCheck(_isClassOrObjCExistential(Element.self))
    _sanityCheck(_isClassOrObjCExistential(U.self))
    
    return _ArrayBuffer<U>(
      storage: _ArrayBridgeStorage(
       native: _native._storage, bits: deferredTypeCheckMask))
  }
  
  @_inlineable
  @_versioned
  internal var needsElementTypeCheck: Bool {
    // 当 element 类型不是 AnyObject 时，NSArry 需要一个 element 类型检查。
    return !_isNativeTypeChecked && !(AnyObject.self is Element.Type)
  }
  
  //===--- private ---===//
  @_inlineable
  @_versioned
  internal init(storage: _ArrayBridgeStorage) {
    _storage = storage
  }
  
  @_versioned
  internal var _storage: _ArrayBridgeStorage
}

extension _ArrayBuffer {
  // 采用 `source` 的 storage
  @_inlineable
  @_versioned
  internal init(_buffer source: NativeBuffer, shiftedToStartIndex: Int) {
    _sanityCheck(shiftedToStartIndex == 0, "shiftedToStartIndex must be 0")
    _storage = _ArrayBridgeStorage(native: source._storage)
  }
  
  @_inlineable
  @_versioned
  internal mutating func isUniquelyReferenced() -> Bool {
    if !_isClassOrObjCExistential(Element.self) {
      return _storage.isUniquelyReferenced_native_noSpareBits()
    }
    return _storage.isUniquelyReferencedNative() && _isNative
  }
  
  // 转换到一个 NSArray
  //
  // 如果 element 类型被一字不差地桥接 O(1)，否则 O(*n*)
  @_inlineable
  @_versioned // FIXME(abi): Used from tests
  internal func _asCocoaArray() -> _NSArrayCore {
    return _fastPath(_isNative) ? _native._asCocoaArray() : _nonNative
  }
  
  @_inlineable
  @_versioned
  internal mutating func requestUniqueMutableBackingBuffer(minimumCapacity: Int)
  -> NativeBuffer? {
    if _fastPath(isUniquelyReferenced()) {
      let b = _native
      if _fastPath(b.capacity >= minimumCapacity) {
        return b
      }
    }
    return nil
  }
  
  @_inlineable
  @_versioned
  
  internal mutating func isMutableAndUniquelyReferenced() -> Bool {
    return isUniquelyReferenced()
  }
  
  @_inlineable
  @_versioned
  internal mutating func isMutableAndUniquelyReferencedOrPinned() -> Bool {
    return isUniquelyReferencedOrPinned()
  }
  
  @_inlineable
  @_versioned
  internal func requestNativeBuffer() -> NativeBuffer? {
    if !_isClassOrObjCExistential(Element.self) {
      return _native
    }
    return _fastPath(_storage.isNative) ? _native : nil
  }
  
  // 我们有两个版本的类型检查：
  @_inlineable
  @_versioned
  @_inline(never)
  internal func _typeCheckSlowPath(_ index: Int) {
    if _fastPath(_isNative) {
      let element: AnyObject = cast(toBufferOf: AnyObject.self)._native[index]
      _precondition(
       element is Element,
       "Down-casted Array element failed to match the target type")
    } else {
      let ns = _nonNative
      _precondition(
       ns.objectAt(index) is Element,
       "NSArray element failed to match the swift Array Element type")
    }
    
    @_inlineable
    @_versioned
    internal func _typeCheck(_ subRange: Range<Int>) {
      if !_isClassOrObjCExistential(Element.self) {
        return
      }
      
      if _slowPath(needsElementTypeCheck) {
        // 可以被加快：例如在 non-native 例子里通过使用 enumerateObjectsAtIndexes:options:usingBlock:
        for i in CountableRange(subRange) {
          _typeCheckSlowPath(i)
        }
      }
    }
    
    @_inlineable
    @_versioned
    @discardableResult
    internal func _copyContents(
   	  subRange bounds: Range<Int>,
      initializing target: UnsafeMutablePointer<Element>
    ) -> UnsafeMutablePointer<Element> {
      _typeCheck(bounds)
      if _fastPath(_isNative) {
        return _native.copyContents(subRange: bounds, initializing: target)
      }
      
      let nonNative = _nonNative
      
      let nsSubRange = SwiftShims._SwiftNSRange(
        location: bounds.loweredBound,
        length: bounds.upperBound - bounds.lowerBound
      )
      
      let buffer = UnsafeMutableRawPointer(target).assumingMemoryBound(to: AnyObject.self)
      
      nonNative.getObjects(buffer, range: nsSubRange)
    }
  }
}
```

