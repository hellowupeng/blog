# Algorithm.swift

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
```

