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
      
      // Copies the references out of the NSArray whithout retaining them
      nonNative.getObjects(buffer, range: nsSubRange)
      
      // Make another pass to retain the copied objects
      var result = target
      for _ in CountableRange(bounds) {
        result.initialize(to: result.pointee)
        result += 1
      }
      return result
    }
    
    // 从这个 buffer 返回包含在 `bounds` 里元素的子区间的一个 `_SliceBuffer`。
    @_inlineable
    @_versioned
    internal subscript(bounds: Range<Int>) -> _SliceBuffer<Element> {
      get {
        _typeCheck(bounds)
        
        if _fastPath(_isNative) {
          return _native(bounds)
        }
        
        let boundsCount = bounds.count
        if boundsCount == 0 {
          return _SliceBuffer(
           _buffer: _ContiguousArrayBuffer<Element>(),
           shiftedToStartIndex: bounds.lowerBound)
        }
        
        let nonNative = self._nonNative
        let cocoa = _CocoaArrayWrapper(nonNative)
        let cocoaStorageBaseAddress = cocoa.contiguousStorage(Range(self.indices))
        
        if let cocoaStorageBaseAddress = cocoaStorageBaseAddress {
          let basePtr = UnsafeMutableRawPointer(cocoaStorageBaseAddress)
           .assumingMemoryBound(to: Element.self)
          return _SliceBuffer(
           owner: nonNative,
           subscriptBaseAddress: basePtr,
           indices: bounds,
           hasNativeBuffer: false)
        }
        
        // 没有发现连续的存储，我们必须分配
        let result = _ContiguousArrayBuffer<Element>(
         _uninitializedCount: boundsCount, minimumCapacity: 0)
        
        cocoa.buffer.getObjects(
         UnsafeMutableRawPointer(result.firstElementAddress)
          .assumingMemoryBound(to: AnyObject.self),
         range: _SwiftNSRange(location: bounds.lowerBound, length: boundsCount))
        
        return _SliceBuffer(
         _buffer: result, shiftedToStartIndex: bounds.lowerBound)
      }
      set {
        fatalError("not implemented")
      }
    }
    
    // 指向第一个元素的指针
    //
    // - 前提：elements 知道将会被连续地存储
    @_inlineable
    @_versioned
    internal var firstElementAddress: UnsafeMutablePointer<Element> {
      _sanityCheck(_isNative, "must be a native buffer")
      return _native.firstElementAddress
    }
    
    @_inlineable
    @_versioned
    internal var firstElementAddressIfContiguous: UnsafeMutablePointer<Element>? {
      return _fastPath(_isNative) ? firstElementAddress : nil
    }
    
    @_inlineable
    @_versioned
    internal var count: Int {
      @inline(__always)
      get {
        return _fastPath(_isNative) ? _native.count : _nonNative.count
      }
      set {
        _sanityCheck(_isNative, "attempting to update count of Cocoa array")
        _natvie.count = newValue
      }
    }
    
    
    
    // Traps if an inout violation is detected or if the buffer
    // is native and the subscript is out of range.
    //
    // wasNative == _isNative in the absence of inout violations.
    // Because the optimizer can hoist the original check it might have been
    // invalidated by illegal code.
    @_inlineable
    @_versioned
    internal func _checkInoutAndNativeBounds(_ index: Int, wasNative: Bool) {
      _precondition(
       _isNative == wasNative,
       "inout rules were violated: the array was overwritten")
      
      if _fastPath(wasNative) {
        _native._checkValidSubscript(index)
      }
    }
    
    @_inlineable
    @_versioned
    internal func _checkInoutAndNativeTypeCheckedBounds(
     _ index: Int, wasNativeTypeChecked: Bool) {
      _precondition(
        _isNative == wasNative,
        "inout rules were violated: the array was overWritten")
      
      if _fastPath(wasNative) {
        _native._checkValidSubscript(index)
      }
    }
    
    @_inlineable
    @_versioned
    internal func _checkInoutAndNativeTypeCheckedBounds(
    _ index: Int, wasNativeTypeChecked: Bool)
  } {
    _precondition(
     _isNativeTypeChecked == wasNativeTypeChecked,
     "inout rules were violated: the array was overwritten")
    
    if _fastPath(wasNativeTypeChecked) {
      _native._checkValidSubscript(index)
    }
  }
  
  @_inlineable
  @_versioned
  internal var capacity: Int {
    return _fastPath(_isNative) ? _native.capacity : _nonNative.count
  }
  
  @_inlineable
  @_versioned
  @inline(__always)
  internal func getElement(_ i: Int, wasNativeTypeChecked: Bool) -> Element {
    if _fastPath(wasNativeTypeChecked) {
      return _nativeTypeChecked[i]
    }
    return unsafeBitCast(_getElementSlowPath(i), to: Element.self)
  }
  
  @_inlineable
  @_versioned
  @inline(never)
  internal func _getElementSlowPath(_ i: Int) -> AnyObject {
    _sanityCheck(
     _isClassOrObjCExistential(Element.self),
     "Only single reference elements can be indexed here.")
    let element: AnyObject
    if _isNative {
      _native._CheckValidSubScript(i)
      
      element = cast(toBufferOf: AnyObject.self)._native[i]
      _precondition(
       element is Element,
       "Down-casted Array element failed to match the target type")
    } else {
      element = _nonNative.objectAt(i)
      _precondition(
       element is Element,
       "NSArray element failed to match the swift Array Element type")
    }
    return element
  }
  
  @_inlineable
  @_versioned
  internal subscript(i: Int) -> Element {
    get {
      return getElement(i, wasNativeTypeChecked: _isNativeTypeChecked)
    }
    
    nonMutating set {
      if _fastPath(_isNative) {
        _native[i] = newValue
      }
      else {
        var refCopy = self
        refCopy.replaceSubrange(
         i..<(i + 1),
         with: 1,
         elementsOf: CollectionOfOne(newValue))
      }
    }
  }
  
  @_inlineable
  @_versioned
  internal func withUnsafeBufferPointer<R>(
   - body: (UnsafeBufferPointer<Element>) throws -> R
  ) rethrows -> R {
    if _fastPath(_isNative) {
      defer { _fixLifetime(self) }
      return try body(
       UnsafeBufferPointer(start: firstElementAddress, count: count))
    }
    return try ContiguousArray(self).withUnsafeBufferPointer(body)
  }
}
```

