# LosslessStringConvertible

###### Protocol

能用无损的，清晰的方式表示为字符串的类型。

## 概览（Overview）

例如，整数值1050能用它的全部被表示为“1050”。

一个符合类型的 description 属性必须是原始值的值保护表示。同样的，他应该能从字符串表示重新创建一个实例。

## 主题（Topics）

### 初始化器（Initializers）

###### init?(String)

从字符串表示实例化一个遵循类型的实例。

###### 必要的

## 关系（Relationships）

### 继承自（Inherit From）

###### CustomStringConvertible

### 继承的类（Inherit By）

###### FixedWidthInteger, StringProtocol

### 采用的类（Adopted By）

- ###### Bool

- ###### Double

- ###### Float

- ###### Float80

- ###### Substring

- ###### Unicode.Scalar

## 另见（See Also）

### 字符串表示（String Representation）

###### protocol CustomStringConvertible

一种可自定义的文本表示类型。

###### protocol CustomDebugStringConvertible

一种适用于调试目的的可自定义的文本表示类型。