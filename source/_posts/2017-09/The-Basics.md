

# The Basics（基础）

Swift 是一种用于iOS，macOS，watchOS 和 tvOS 应用开发的新语言。尽管如此，Swift 的许多部分会和你使用 C 和 Objective-C 的开发体验面熟。

Swift 为所有基础的 C 和 Objective-C 类型提供了自己的版本，包括对于整型的 `Int`，对于浮点值的 `Double` 和 `Float`，对于布尔值的 `Bool` 和 对于文本数据的 `String`。Swift 也提供了三种主要集合类型，`Array` 、 `Set` 和 `Dictionary`，在集合类型（Collection Types）里有描述。

像 C 一样，Swift 使用变量来存储并通过一个标识名来引用值。Swift 也广泛使用值不能改变的变量。也就是大家所熟知的常量，比 C 里面的常量更加强大。在整个 Swift 中，当你使用不需要改变的值的时候，常量被用来让代码更加安全和清晰。

除了熟悉的类型以外，Swift 还介绍了在 Objective-C 里面没有的高级类型，例如元组（tuples）。元组使你能够创建并传递一组值。你可以使用元组作为一个单一的复合值从一个函数返回多个值。

Swift 也介绍了处理值缺失的可选型。可选型表示要么“这里有值，它等于x”，要么“这里根本没有值”。使用可选型类似于在 Objective-C 里对于指针使用 `nil` ，但是它们适用于任何类型，不仅仅是类。