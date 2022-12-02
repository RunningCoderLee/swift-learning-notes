# 类和结构体

## 结构体和类对比

### 相同点

* 定义属性用于存储值
* 定义方法用于提供功能
* 定义下标操作用于通过下标语法访问它们的值
* 定义构造器用于设置初始值
* 通过扩展以增加默认实现之外的功能
* 遵循协议以提供某种标准功能

### 不同点

类具有如下附件功能

* 继承允许一个类继承另一个类的特征
* 类型转换允许在运行时检查和解释一个类实例的类型
* 析构器允许一个类实例释放任何其所被分配的资源
* 引用计数允许对一个类的多次引用

### 类型定义的语法

通过关键字 `struct` 引入结构体，通过 `class` 关键字引入类

```swift
struct SomeStructure {
    // 在这里定义结构体
}
class SomeClass {
    // 在这里定义类
}
```

> **注意：** 使用 `UpperCamelCase` 来命名类型，使用 `lowerCamelCase` 来命名属性和方法

```swift
struct Resolution {
    var width = 0
    var height = 0
}
class VideoMode {
    var resolution = Resolution()
    var interlaced = false
    var frameRate = 0.0
    var name: String?
}
```

### 结构体和类的实例

```swift
let someResolution = Resolution()
let someVideoMode = VideoMode()
```

### 属性访问

通过 `.` 语法访问实例的属性

```swift
print("The width of someResolution is \(someResolution.width)")
// 打印 "The width of someResolution is 0"

someVideoMode.resolution.width = 1280
print("The width of someVideoMode is now \(someVideoMode.resolution.width)")
// 打印 "The width of someVideoMode is now 1280"
```

### 结构体类型的成员逐一构造器

所有结构体都有一个自动生成的成员逐一构造器，用于初始化新结构体实例中成员的属性。新实例中各个属性的初始值可以通过属性的名称传递到成员逐一构造器之中：

```swift
let vga = Resolution(width: 640, height: 480)
```

## 结构体和枚举是值类型

> **注意：** Swift 中所有的基本类型：整数（integer）、浮点数（floating-point number）、布尔值（boolean）、字符串（string)、数组（array）和字典（dictionary），都是值类型，其底层也是使用结构体实现的。**所有结构体和枚举类型都是值类型**



## 类是引用类型

与值类型不同，引用类型在被赋予到一个变量、常量或者被传递到一个函数时，其值不会被拷贝。因此，使用的是已存在实例的引用，而不是其拷贝。

### 恒等运算符

使用恒等运算符可以检测两个常量或者变量是否引用了同一个实例：

* 相同（`===`）
* 不相同（`!==`）


### 指针

TODO
