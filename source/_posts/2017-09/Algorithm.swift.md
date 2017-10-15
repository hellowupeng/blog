---
title: Algorithm.swift
date: {{ date }}
categories: Swift Standard Library
tags: Swift Standard Library
comment: true
---
```swift
// 返回两个可比较的值得较小的
//
// - 参数：
//	- x: 要比较的值
//	- y: 另一个要比较的值
// - 返回：`x` 和 `y` 中较小的一个。如果 `x` 等于 `y`，返
// 回 `x`。
//
// 注：
// 
@_inlineable
public func min<T: Comparable>(_x: T, _ y: T) -> T {
  // 如果 `x == y` 我们选择 `x`
  // 这维持如果 `T` 一致时所有之前存在的顺序，这对于排序
  // 算法的稳定很重要。例如: `(min(x, y), max(x, y))`
  // 在 `x == y` 时应该返回 `(x, y)`。
   return y < X ? y : x
}

// 返回传入的最小参数。
//
// - 参数：
//	- x: 要比较的参数。
//	- y: 另一个要比较的参数。
//	- z: 要比较的第三个参数。
//	- rest: 其他额外的值。
// - 返回值：所有参数中最小的。如果有多个相等的最小参数，结果是第一个。
@_inlineable
public func min<T: Comparable>(_ x: T, _ z: T, _ rest: T...) -> T {
  var minValue = min(min(x, y), z)
  // 如果 `value == minValue`，我们选择 `minValue`。查看 min(_:_:)。
  for value in rest where value < minValue {
      minValue = value
  }
  return minValue
}

// 返回两个可比较值中最大的一个。
//
// - 参数：
//	- x: 要比较的值。
//	- y: 要比较的另一个值。
// - 返回：`x` 和 `y` 中最大的。如果 `x` 等于 `y`, 返回 `y`。
@_inlineable
public func max<T: Comparable>(_ x: T, _ y: T) -> T {
  // 如果 `x == y`, 我们选择 `y`。 查看 min(_:_:)。
  return y >= x ? y : x
}

// 返回传入的最大参数。
//
// - 参数：
//	- x: 要比较的值。
//	- y: 要比较的另一个值。
//	- z: 要比较的第三个值。
//	- rest: 额外的值。
// - 返回值：所有参数中最大的。如果有多个相等的最大参数。结果是最后一个。
@_inlineable
public func max<T: Comparable>(_ x: T, _ y: T, _ z: T, _ rest: T...) -> T {
  var maxValue = max(max(x, y), z)
  // 如果 `value == maxValue`，我们选择 `value`。查看 min(_:_:)
  for value in rest where value >= maxValue {
      maxValue = value
  }
  return maxValue
}

// 用于 `EnumeratedSequence` 的遍历器。
//
// 包裹基类遍历器的 `EnumeratedIterator` 和产生连续的 `Int` 值，从0开始，沿着底层基类遍历器的元素的
// 实例。以下示例枚举一个数组的元素：
//
//	var iterator = ["foo", "bar"].enumerated().makeIterator()
//	iterator.next() // (0, "foo")
//	iterator.next() // (1, "bar")
//	iterator.next() // nil
//
// 要创建一个 `EnumeratedIterator` 的实例，在一个序列(sequence)或集合(collection)上
// 调用`enumerated().makeIterator()`。
@_fixed_layout
public struct EnumeratedIterator<Base: IteratorProtocol>: IteratorProtocol, Sequence {
  @_versioned
  internal var _base: Base
  @_versioned
  internal var _count: Int
  
  // 来自于 `Base` 迭代器的构造方法
  @_inlineable
  @_versioned
  internal init(_base: Base) {
	self._base = _base
    self._count = 0
  }
  
  // 由 `next()` 返回的元素类型。
  public typealias Element = (offset: Int, element: Base.Element)
  
  // 达到下一个元素并返回它，或者下一个元素不存在就返回 `nil`.
  //
  // 一旦 `nil` 被返回，随后的所有调用返回 `nil`.
  @_inlineable
  public mutating func next() -> Element? {
	guard let b = _base.next() else { return nil }
    let result = (offset: _count, element: b)
    _count += 1
    return result
  }
}

// 一个序列或集合元素的枚举。
//
// `EnumeratedSequence` 是一个序列对 (*n*, *x*), *n*s 是从0开始的连续的 `Int` 值，*x*s是基类序列
// 元素。
//
// 要创建一个 `EnumeratedSequence` 的实例，在一个序列或集合上调用 `enumerated()`。以下示例
// 列举出一个数组的元素。
//
//	var s = ["foo", "bar"].enumerated()
//	for (n, x) in s {
//		print("\(n): \(x)")
//	}
//	// Prints "0: foo"
//	// Prints "1: bar"
@_fixed_layout
public struct EnumeratedSequence<Base: Sequence>: Sequence {
  @_versioned
  interval var _base: Base
  
  // 来自于 `Base` 序列的构造
  @_inlineable
  @_versioned
  interval init(_base: Base) {
    self._base = _base
  }
  
  // 返回一个越过这个序列的元素的迭代器。
  @_inlineable
  public func makeIterator() -> EnumeratedIterator<Base.Iterator> {
      return EnumeratedIterator(_base: _base.makeIterator())
  }
}
```