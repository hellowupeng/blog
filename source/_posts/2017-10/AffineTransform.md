```swift
#if os(OSX) || os(iOS)
	import Darwin
#elseif os(Linux) || CYGWIN
	import Glibc
#endif

private let ε: CGFloat = CGFloat(2.22045e-16)

/**
 AffineTransform 表示一个以下形式的仿射变换矩阵：

 [ m11 m12 0 ]
 [ m21 m22 0 ]
 [  tX  tY 1 ]
*/
public struct AffineTransform : ReferenceConvertible, Hashable, CustomStringConvertible {
    public typealias ReferenceType = NSAffineTransform
  
  	public var m11: CGFloat
  	public var m12: CGFloat
  	public var m21: CGFloat
  	public var m22: CGFloat
  	public var tX: CGFloat
  	public var tY: CGFloat
  
  	// 用标识值创建一个仿射变换矩阵。
  	public init() {
        self.init(m11: 1.0, m12: 0.0,
                  m21: 0.0, m22: 1.0,
                   tX: 0.0,  tY: 0.0)
    }
  
  	// 创建一个仿射变换。
  	public init(m11: CGFloat, m12: CGFloat, m21: CGFloat, m22: CGFloat, tX: CGFloat,
               tY: CGFloat) {
        (self.m11, self.m12, self.m21, self.m22) = (m11, m12, m21, m22)
      	(self.tX, self.tY) = (tX, tY)
    }
  
  	/**
  	 从 translation values 创建仿射变换矩阵。
  	 这个矩阵采用以下形式：
  
  	 [ 1 0 0 ]
  	 [ 0 1 0 ]
  	 [ x y 1 ]
  	*/
  	public init(translationByX x: CGFloat, byY y: CGFloat) {
        self.init(m11: CGFloat(1.0), m12: CGFloat(0.0),
                  m21: CGFloat(0.0), m22: CGFloat(1.0),
                   tX: x,			  tY: y)
    }
  
  	/**
  	 从 scaling values 创建仿射变换矩阵。
  	 矩阵采用以下形式：
  	
  	 [ x 0 0 ]
  	 [ 0 y 0 ]
  	 [ 0 0 1 ]
  	*/
  	public init(scaleByX x: CGFloat, byY y: CGFloat) {
        self.init(m11: x, 			 m12: CGFloat(0.0),
                  m21: CGFloat(0.0), m22: y,
                   tX: CGFloat(0.0),  tY: CGFloat(0.0))
    }
  
  	/**
  	 从 scaling a single value 创建一个仿射变换矩阵。
  	 矩阵采用以下形式：
  
  	 [ f 0 0 ]
  	 [ 0 f 0 ]
  	 [ 0 0 1 ]
  	*/
  	public init(scale factor: CGFloat) {
        self.init(scaleByX: factor, byY: factor)
    }
  
  	/**
  	 从 rotation value (以弧度计量的角度)创建一个仿射变换矩阵。
  	 矩阵采用以下形式：
  
  	 [  cos α sin α  0 ]
  	 [ -sin α cos α  0 ]
  	 [    0      0	 1 ]
  	*/
  	public init(rotatingByRadians angle: CGFloat) {
        let sine = sin(angle)
      	let cosine = cos(angle)
      
      	self.init(m11: cosine, m12: sine, m21: -sine, m22: cosine, tX: CGFloat(0.0),
                   tY: CGFloat(0.0))
    }
  
  	/**
  	 从一个旋转值（以度计量的角度）创建一个仿射变换矩阵。
  	 矩阵采用以下形式。
  
  	 [ cos α sin α 0 ]
  	 [ -sin α cos α 0 ]
  	 [ 0 	0		1 ]
  	*/
  	public init(rotationByDegrees angle: CGFloat) {
        let α = angle * .pi / 180.0
      	self.init(rotationByRadians: α)
    }
  
  	/**
  	 一个标识仿射变换矩阵。
  
  	 [ 1 0 0 ]
  	 [ 0 1 0 ]
  	 [ 0 0 1 ]
  	*/
  	public static let identity = AffineTransform(m11: CGFloat(1.0), m12: CGFloat(0.0),
 		m21: CGFloat(0.0), m22: CGFloat(1.0), tX: CGFloat(0.0), tY: CGFloat(0.0))
  
  	// 转换
  	public mutating func translate(x: CGFloat, y: CGFloat) {
        tX += m11 * x + m21 * y
      	tY += m12 * x + m22 * y
    }
  
  	/**
  	 从一个旋转值(以度数计量的角度 α)改变一个仿射变换矩阵。
  	 矩阵采用以下形式：
  
  	 [ cos α	sin α	0 ]
  	 [ -sin α	cos α	0 ]
  	 [ 	 0		  0		1 ]
  	*/
  	public mutating func rotate(byDegrees angle: CGFloat) {
        let α = angle * .pi / 180.0
      	return rotate(byRadians: α)
    }
  
  	public mutating func rotate(byRadians angle: CGFloat) {
        let sine = sin(angle)
      	let cosine = cos(angle)
      
      	m11 = cosine
      	m12 = sine
      	m21 = -sine
      	m22 = cosine
    }
}
```

