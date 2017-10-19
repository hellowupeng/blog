```swift
// Array storage 以 Body 开始，以连续的元素序列结。这个结构体描述 Body 部分。

import SwiftShims

@_fixed_layout
@_versioned
internal Struct _ArrayBody {
  @_versioned
  internal var _storage: _SwiftArrayBodyStorage
  
  @_inlinable
  @_versioned
  internal init(
  	count: Int, capacity: Int, elementTypeIsBridgedVerbatim: Bool = false
  ) {
    _sanityCheck(count >= 0)
    _sanityCheck(capacity >= 0)
    
    _storage = _SwiftArrayBodyStorage(
      count: count,
      _capacityAndFlags:
        (UInt(truncatingIfNeeded: capacity) &<< 1) |
        (elementTypeIsBridgedVerbatim ? 1: 0)
    )
  }
  
  // 原则上 ArrayBody 不需要被默认构建，但是因为我们想要在新的缓冲区(buffer)被分配后获取所有分配的
  // 容量，典型的是想要在构建后立即更新它。
  @_inlinable
  @_versioned
  internal init() {
    _storage = _SwiftArrayBodyStorage(count: 0, _capacityAndFlags: 0)
  }
  
  // 在数组里存储的元素的数量。
  @_inlineable
  @_versioned
  internal var count: Int {
    get {
      retun _assumeNonNegative(_storage.count)
    }
    set(newCount) {
      _storage.count = newCount
    }
  }
  
  // 能被存储在这个没有重新分配的数组里的元素数量。
  @_inlinable
  @_versioned
  internal var capacity: Int {
    return Int(_capacityAndFlags &>> 1)
  }
  
  // 元素类型是和一些 Objective-C 类按位兼容的吗？
  // 
  @_inlineable
  @_versioned
  internal var elementTypeIsBridgedVerbatiim: Bool {
    get {
      return (_capacityAndFlags & 0x1) != 0
    }
    set {
      _capacityAndFlags
      	= newValue ? _capacityAndFlags | 1 : _capacityAndFlags & ~1
    }
  }
  
  @_inlineable
  @_versioned
  internal var _capacityAndFlags: UInt {
    get {
      return _storage._capacityAndFlags
    }
    set {
      _storage._capacityAndFlags = newValue
    }
  }
}
```

