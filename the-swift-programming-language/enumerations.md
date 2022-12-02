# 枚举

## 语法

```swift
enum SomeEnumeration {
    // 枚举定义放在这里
} 
```

例子：

```swift
enum CompassPoint {
    case north
    case south
    case east
    case west
}
```

> **注意：** 与 C 和 Objective-C 不同，Swift 的枚举成员在被创建时不会被赋予一个默认的整型值。在上面的 CompassPoint 例子中，north，south，east 和 west 不会被隐式地赋值为 0，1，2 和 3。相反，这些枚举成员本身就是完备的值，这些值的类型是已经明确定义好的 CompassPoint 类型。

多个成员值可以在同一行上

```swift
enum Planet {
    case mercury, venus, earth, mars, jupiter, saturn, uranus, neptune
}
```

## 使用 Switch 语句匹配枚举值

```swift
directionToHead = .south
switch directionToHead {
case .north:
    print("Lots of planets have a north")
case .south:
    print("Watch out for penguins")
case .east:
    print("Where the sun rises")
case .west:
    print("Where the skies are blue")
}
// 打印“Watch out for penguins”
```

**注意：** 在判断一个枚举类型的值时，`switch` 语句必须穷举所有情况，当不需要匹配每个枚举成员的时候，你可以提供一个 `default` 分支来涵盖所有未明确处理的枚举成员：

```swift
let somePlanet = Planet.earth
switch somePlanet {
case .earth:
    print("Mostly harmless")
default:
    print("Not a safe place for humans")
}
// 打印“Mostly harmless”
```

## 枚举成员的遍历

通过让声明的枚举遵循 `CaseIterable` 协议，从而生成一个 `allCases` 属性，用于表示一个包含枚举所有成员的集合

```swift
enum Beverage: CaseIterable {
    case coffee, tea, juice
}
let numberOfChoices = Beverage.allCases.count
print("\(numberOfChoices) beverages available")
// 打印“3 beverages available”

for beverage in Beverage.allCases {
    print(beverage)
}
// coffee
// tea
// juice
```

## 关联值

可以定义 Swift 枚举来存储任意类型的关联值，如果需要的话，每个枚举成员的关联值类型可以各不相同。枚举的这种特性跟其他语言中的可识别联合（discriminated unions），标签联合（tagged unions），或者变体（variants）相似。

```swift
enum Barcode {
    case upc(Int, Int, Int, Int)
    case qrCode(String)
}

var productBarcode = Barcode.upc(8, 85909, 51226, 3)

productBarcode = .qrCode("ABCDEFGHIJKLMNOP")
```

可以使用一个 switch 语句来检查不同的条形码类型

```swift
switch productBarcode {
case .upc(let numberSystem, let manufacturer, let product, let check):
    print("UPC: \(numberSystem), \(manufacturer), \(product), \(check).")
case .qrCode(let productCode):
    print("QR code: \(productCode).")
}
// 打印“QR code: ABCDEFGHIJKLMNOP.”

// 简化版
switch productBarcode {
case let .upc(numberSystem, manufacturer, product, check):
    print("UPC: \(numberSystem), \(manufacturer), \(product), \(check).")
case let .qrCode(productCode):
    print("QR code: \(productCode).")
}
// 打印“QR code: ABCDEFGHIJKLMNOP.”
```


## 原始值

```swift
enum ASCIIControlCharacter: Character {
    case tab = "\t"
    case lineFeed = "\n"
    case carriageReturn = "\r"
}
```

### 原始值的隐式赋值

在使用原始值为整数或者字符串类型的枚举时，不需要显式地为每一个枚举成员设置原始值，Swift 将会自动为你赋值。


```swift
enum Planet: Int {
    case mercury = 1, venus, earth, mars, jupiter, saturn, uranus, neptune
}
// venus = 2
// ...
// neptune = 8
```

**当使用字符串作为枚举类型的原始值时，每个枚举成员的隐式原始值为该枚举成员的名称。**

```swift
enum CompassPoint: String {
    case north, south, east, west
}
```

使用 `rawValue` 可以访问该枚举成员的原始值

```swift
let earthsOrder = Planet.earth.rawValue
// earthsOrder 值为 3

let sunsetDirection = CompassPoint.west.rawValue
// sunsetDirection 值为 "west"
```

### 使用原始值初始化枚举实例

如果在定义枚举类型的时候使用了原始值，那么将会自动获得一个初始化方法，这个方法接收一个叫做 rawValue 的参数，参数类型即为原始值类型，返回值则是枚举成员或 nil。你可以使用这个初始化方法来创建一个新的枚举实例。

```swift
let possiblePlanet = Planet(rawValue: 7)
// possiblePlanet 类型为 Planet? 值为 Planet.uranus
```

原始值构造器总是返回一个**可选的**枚举成员。

## 递归枚举

通过在成员之前或者枚举类型之前添加 `indirect` 关键字来声明该类型成员或整个枚举的所有成员都是可递归的

```swift
enum ArithmeticExpression {
    case number(Int)
    indirect case addition(ArithmeticExpression, ArithmeticExpression)
    indirect case multiplication(ArithmeticExpression, ArithmeticExpression)
}

indirect enum ArithmeticExpression {
    case number(Int)
    case addition(ArithmeticExpression, ArithmeticExpression)
    case multiplication(ArithmeticExpression, ArithmeticExpression)
}

// 使用案例
let five = ArithmeticExpression.number(5)
let four = ArithmeticExpression.number(4)
let sum = ArithmeticExpression.addition(five, four)
let product = ArithmeticExpression.multiplication(sum, ArithmeticExpression.number(2))
```

要操作具有递归性质的数据结构，使用递归函数是一种直截了当的方式。例如，下面是一个对算术表达式求值的函数：

```swift
func evaluate(_ expression: ArithmeticExpression) -> Int {
    switch expression {
    case let .number(value):
        return value
    case let .addition(left, right):
        return evaluate(left) + evaluate(right)
    case let .multiplication(left, right):
        return evaluate(left) * evaluate(right)
    }
}

print(evaluate(product))
// 打印“18”
```