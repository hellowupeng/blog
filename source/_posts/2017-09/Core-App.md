---
title: Core App（核心应用）
date: {{ date }}
categories: Apple文档
tags: iOS
comment: true
---
[原文地址](https://developer.apple.com/documentation/uikit/core_app)

管理你的应用的数据模型和它与系统的交互。

## 主题（Topics）

### 应用（Application）

##### [保护用户隐私]()

通过加密个人数据和尊重用户如何使用数据的意愿保护用户隐私

`class` [UIApplication]()

运行在iOS上的应用的控制和协调的集中点。

`protocol` [UIApplicationDelegate]()

由单例 `UIApplication` 对象调用的方法集合，响应在你的应用的生命周期里的重要事件。

`func` [UIApplicationMain]()( Int32, UnsafeMutablePointer<UnsafeMutablePointer<Int8>>!, String?, String?)

创建应用对象和应用代理并设置事件循环（event cycle）。

### 设备环境（Device Environment）

##### [响应在 Apple TV 上改变显示模式]()

当在你设备上的整个屏幕改变时动态地改变图片和资源。

`class` [UIDevice]()

当前设备的描述。

`class` [UITraitCollection](https://developer.apple.com/documentation/uikit)

由 trait 定义，用于你的应用的 iOS 界面环境，例如水平和垂直尺寸类（size class），显示缩放和用户界面语言。

`protocol` [UITraitEnvironment](https://developer.apple.com/documentation/uikit)

使 iOS 界面环境可用于你的应用的方法集合。

`protocol` [UIAdaptivePresentationControllerDelegate](https://developer.apple.com/documentation/uikit)

连同呈现控制器（presentation controller），决定如何响应你的应用里的特征改变的方法集合。

### 文档（Documents）

`class` [UIDocument](https://developer.apple.com/documentation/uikit)

用于管理你的应用的数据的分离部分的抽象基类。

`class` [UIManagedDocument](https://developer.apple.com/documentation/uikit)

与 Core Data 整合的托管的文档对象。

### 粘贴板（Pasteboard）

`class` [UIPasteboard](https://developer.apple.com/documentation/uikit)

帮助用户从你的应用里的一个地方到另一个地方，从你的应用到其他应用分享数据的对象。

`class` [UIPasteConfiguration](https://developer.apple.com/documentation/uikit)

一个对象实现声明它的能力来接收特定的数据类型用于粘贴和用于拖放活动的接口。

`protocol` [UIPasteConfigurationSupporting](https://developer.apple.com/documentation/uikit)

决定一个响应器对象是否支持粘贴配置的接口。

### 数据管理（Data Management）

`protocol` [UIDataSourceModelAssociation](https://developer.apple.com/documentation/uikit)

定义一个接口用于在你的应用里对数据对象提供持久引用的方法集合。

### 指导访问（Guided Access）

`protocol` [UIGuidedAccessRestrictionDelegate](https://developer.apple.com/documentation/uikit)

在 iOS 里你用来为指导访问特性添加自定义限制的方法集合。

`func` [UIGuidedAccessRestrictionStateForIdentifier](https://developer.apple.com/documentation/uikit)(Striing)

为特定的指导访问限制返回限制状态。

## 另见（See Also）

### 应用结构（App Structure）

##### [资源管理（Resource Management）](https://developer.apple.com/documentation/uikit)

管理你在你的主可执行文件（main executable）外保存的图片，字符串，故事板（storyboards）和 nib 文件。

##### [应用拓展（App Extensions）](https://developer.apple.com/documentation/uikit)

拓展你的应用的基础功能到系统的其他部分。