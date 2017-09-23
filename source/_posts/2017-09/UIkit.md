# UIKit

[原文地址](https://developer.apple.com/documentation/uikit)

为你的 iOS 或 tvOS 应用构建并管理一个图形的、事件驱动的用户界面。

## 概览（Overview）

UIKit 框架为你的 iOS 和 tvOS 应用提供必需的基础实施。它提供了实现你的界面的窗口（window）和视图（view）基础设施，传递多点触控（Multitouch）和其他输入类型到你应用的事件处理基础设施和需要在用户之间管理交互的主运行循环（main run loop）。由框架提供的其他功能包括动画支持，文档支持，绘制和输出支持，关于当前设备信息，文本管理和显示，搜索支持，无障碍功能支持，应用拓展支持和资源管理。

> ##### 重要
>
> 除非另外说明，只在你的应用的主线程（main thread）或主调度队列（main dispatch queue）使用 UIKit 类。这个限制特别应用于起源自 UIResponder 或涉及以任何方式操作你的应用的用户界面的类。

## 主题（Topics）

### 应用结构（App Structure）

UIKit 管理你的应用和系统的交互并为你提供类来管理你的应用的数据和资源。

##### [核心应用（Core App）]()

管理你的应用的数据模型以及他和系统的交互。

##### [资源管理（Resource Management）]()

管理你在你的主要的可执行文件外存储的图片、字符串、storyboards 和 nib 文件。

##### [应用拓展（App Extensions）]()

拓展你的应用的基础功能到系统的其他部分。

### 用户界面（User Interface）

视图（Views）帮助你在屏幕上显示内容和促进用户交互；视图控制器（view controllers）帮助你管理视图（views）和界面结构。

##### [视图和控件（Views and Controls）]()

在屏幕上呈现内容和定义该内容所允许的交互。

##### [视图管理（View Management）]()

使用视图控制器管理你的界面和促进在不同内容的屏幕之间的导航。

##### [系统视图控制器（System View Controller）]()

使用内置的 UIKit 视图控制器来选择图片，编辑视频，分享内容，打印文件等。

##### [拖放（Drag and Drop）]()

通过与你的视图（views）一起使用交互 API 把拖放功能带到你的应用。

##### [无障碍（Accessibility）]()

使你的应用对于残疾用户更友好。

##### [动画与触觉（Animation and Haptics）]()

使用基于视图的动画和触觉向用户提供反馈。

##### [窗口与屏幕（Windows and Screens）]()

为你的视图层级和其他内容提供一个容器。

### 事件处理（Event Handling）

响应器和手势识别器帮助你处理多点触控，按钮点击，3D Touch事件，键盘输入，自定义输入和自定义动作。

##### [理解事件处理，响应器和响应器链（Understanding Event Handling， Responders，and the Responder Chain）]()

学习事件是如何被传送到你的应用和你如何处理它们。

##### [触摸，按压和手势（Touches，Presses，and Gestures）]()

在手势识别器里封装你的应用的事件处理逻辑，以便你能在整个应用里重用这部分代码。

##### [Peek and Pop]()

使用 3D Touch 输入为你的内容显示自定义预览和动作。

##### [键盘和菜单（Keyboard and Menus）]()

处理键盘输入并显示一个自定义动作菜单。

### 图形，绘制和输出（Graphics，Drawing，and Printing）

UIKit 提供类和协议帮助你配置你的绘制环境和渲染内容。

##### [图片和 PDF（Images and PDF）]()

创建并管理图片，包括哪些使用位图（bitmap）和 PDF 格式的图片。

##### [绘制（Drawing）]()

使用渲染器，绘制路径，线，阴影来配置你的应用的绘制环境。

##### [打印（Printing）]()

显示系统打印面板和管理过程。

### 文本（Text）

除了在你的应用里易于显示文本的文本视图（text views）外，UIKit 提供了支持系统键盘的自定义文本管理和渲染。

##### [文本显示和字体（Text Display and Fonts）]()

使用 UIKit 视图显示文本，管理字体和检查拼写。

##### [文本存储（Text Storage）]()

管理文本存储和协调文本的布局。

##### [键盘和输入（Keyboards and Input）]()

配置系统键盘或创建你自己的键盘并自己处理输入。

### 弃用（Deprecated）

避免在你的应用里使用弃用的类和协议。

##### [弃用的符号（Deprecated Symbols）]()

### 结构体（Structures）

`struct`  [UIDocumentBrowserError]()

一个包含关于文档浏览错误信息的结构体。

### 类（Classes）

`class`  [UICollectionViewDropPlaceholder]()

`class` [UICollectionViewPlaceholder]()

`class` [UITableViewDropPlaceholder]()

`class` [UITableViewPlaceholder]()

### 协议（Protocols）

`protocol` [UIItemProviderPresentationSizeProviding]()

### 参考（Reference）

##### [UIKit Enumerations]()