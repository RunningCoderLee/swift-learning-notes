
- [基础部分](#基础部分)
  - [数据类型](#数据类型)
  - [常量和变量](#常量和变量)
    - [声明](#声明)
    - [类型注解](#类型注解)
    - [命名](#命名)
  - [注释](#注释)
  - [整数](#整数)
    - [范围](#范围)
      - [Int](#int)
      - [Uint](#uint)
      - [浮点数](#浮点数)
  - [数字值字面量](#数字值字面量)
  - [类型别名](#类型别名)
  - [元祖（tuples）](#元祖tuples)
  - [可选类型（Optional）](#可选类型optional)
    - [可选值的强制解析（forced unwrapping）](#可选值的强制解析forced-unwrapping)
    - [可选绑定（optional binding）](#可选绑定optional-binding)
  - [错误处理](#错误处理)
    - [使用断言调试](#使用断言调试)

# 基础部分

## 数据类型
* Int
* Uint(尽量不用，用 Int)
* Double
* Float
* Bool
* String
* Array
* Set
* Dictionary

## 常量和变量
### 声明
* `var` 声明变量
* `let` 声明常量

### 类型注解
可写可不写，系统会自动推断

```swift
var welcomeMessage: String?
```

### 命名
不能包含数学符号，箭头，保留的（或者非法的）Unicode 码位，连线与制表符。也不能以数字开头，但是可以在常量与变量名的其他地方包含数字。

## 注释
```swift
// 这是一个注释

/* 这也是一个注释，
但是是多行的 */

/* 这是第一个多行注释的开头
/* 这是第二个被嵌套的多行注释 */
这是第一个多行注释的结尾 */
```

## 整数
有8、16、32、64位有符号和无符号整数

### 范围

```swift
let minValue = UInt8.min  // minValue 为 0，是 UInt8 类型
let maxValue = UInt8.max  // maxValue 为 255，是 UInt8 类型
```

#### Int
* 在32位平台上，`Int` 和 `Int32` 长度相同 `-2,147,483,648 ~ 2,147,483,647`。
* 在64位平台上，`Int` 和 `Int64` 长度相同。

#### Uint
* 在32位平台上，UInt 和 UInt32 长度相同。
* 在64位平台上，UInt 和 UInt64 长度相同。

#### 浮点数

* Double 表示64位浮点数。当你需要存储很大或者很高精度的浮点数时请使用此类型。
* Float 表示32位浮点数。精度要求不高的话可以使用此类型。

`Double` 精确度很高，至少有 `15` 位小数，而 `Float` 只有 `6` 位小数。优先选择 `Double`

## 数字值字面量

* 一个十进制数，没有前缀
* 一个二进制数，前缀是 0b
* 一个八进制数，前缀是 0o
* 一个十六进制数，前缀是 0x

```swift
let decimalInteger = 17
let binaryInteger = 0b10001       // 二进制的17
let octalInteger = 0o21           // 八进制的17
let hexadecimalInteger = 0x11     // 十六进制的17
```

## 类型别名

关键字 `typealias`

用法

```swift
typealias AudioSample = UInt16

var maxAmplitudeFound = AudioSample.min
// maxAmplitudeFound 现在是 0
```

## 元祖（tuples）
把多个值组合成一个复合值。元组内的值可以是任意类型，并不要求是相同类型。

```swift
let http404Error = (404, "Not Found")
// http404Error 的类型是 (Int, String)，值是 (404, "Not Found")
```
可以将一个元组的内容分解（decompose）成单独的常量和变量

```swift
let (statusCode, statusMessage) = http404Error
print("The status code is \(statusCode)")
// 输出“The status code is 404”
print("The status message is \(statusMessage)")
// 输出“The status message is Not Found
```

通过下标来访问元组中的单个元素，下标从零开始

```swift
print("The status code is \(http404Error.0)")
// 输出“The status code is 404”
print("The status message is \(http404Error.1)")
// 输出“The status message is Not Found”
```

定义元祖的时候给耽搁元素命名，并可通过名字来获取元素值
```swift
let http200Status = (statusCode: 200, description: "OK")

print("The status code is \(http200Status.statusCode)")
// 输出“The status code is 200”
print("The status message is \(http200Status.description)")
// 输出“The status message is OK”
```

## 可选类型（Optional）

```swift
var serverResponseCode: Int? = 404
// serverResponseCode 包含一个可选的 Int 值 404
serverResponseCode = nil
// serverResponseCode 现在不包含值

var surveyAnswer: String?
// surveyAnswer 被自动设置为 nil
```

### 可选值的强制解析（forced unwrapping）
当你确定可选类型确实包含值之后，你可以在可选的名字后面加一个感叹号（!）来获取值

```swift
if convertedNumber != nil {
    print("convertedNumber has an integer value of \(convertedNumber!).")
}
// 输出“convertedNumber has an integer value of 123.”
```

### 可选绑定（optional binding）
可选绑定可以用在 `if` 和 `fwhile` 语句中，这条语句不仅可以用来判断可选类型中是否有值，同时可以将可选类型中的值赋给一个常量或者变量

```swift
if let constantName = someOptional {
    statements
}
```

如果你在访问它包含的值后不需要引用原来的可选常量或是可选变量，你可以对新的常量或是新的变量使用相同的名称：

```swift
let myNumber = Int(possibleNumber)
// 此处 myNumber 为一可选整型
if let myNumber = myNumber {
	// 此处 myNumber 为一不可选整型
	print("My number is \(myNumber)")
}
// 输出 "My number is 123"
```

可简写为
```swift
if let myNumber{
	print("My number is \(myNumber)")
}
// 输出 "My number is 123"
```

## 错误处理

在函数声明中添加 `throws` 关键词来抛出错误消息

```swift
func canThrowAnError() throws {

// 这个函数有可能抛出错误

}
```

使用的时候在表达式中前置 `try` 关键词

```swift
do {
    try canThrowAnError()
    // 没有错误消息抛出
} catch {
    // 有一个错误消息抛出
}
```


处理不同错误条件的例子

```swift
func makeASandwich() throws {
    // ...
}

do {
    try makeASandwich()
    eatASandwich()
} catch SandwichError.outOfCleanDishes {
    washDishes()
} catch SandwichError.missingIngredients(let ingredients) {
    buyGroceries(ingredients)
}
```

### 使用断言调试

TODO:











