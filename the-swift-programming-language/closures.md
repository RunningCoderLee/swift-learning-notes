# 闭包

闭包是自包含的函数代码块，可以在代码中被传递和使用。`Swift` 中的闭包与 `C` 和 `Objective-C` 中的代码块（`blocks`）以及其他一些编程语言中的匿名函数（`Lambdas`）比较相似。

## 表达式语法

闭包表达式语法有如下的一般形式：

```swift
{ (parameters) -> return type in
    statements
}
```

闭包表达式参数 可以是 in-out 参数，但不能设定默认值。如果你命名了可变参数，也可以使用此可变参数。元组也可以作为参数和返回值。

```swift
let names = ["Chris", "Alex", "Ewa", "Barry", "Daniella"]

var eversedNames = names.sorted(by: { (s1: String, s2: String) -> Bool in
    return s1 > s2
})

// 简化版
reversedNames = names.sorted(by: { (s1: String, s2: String) -> Bool in return s1 > s2 } )
```

闭包的函数体部分由关键字 in 引入。该关键字表示闭包的参数和返回值类型定义已经完成，闭包函数体即将开始。

### 根据上下文推断类型

可省略参数和返回值的类型

```swift
reversedNames = names.sorted(by: { s1, s2 in return s1 > s2 } )
```

### 单表达式闭包的隐式返回

单行表达式可省略 `return`

```swift
reversedNames = names.sorted(by: { s1, s2 in s1 > s2 } )
```

### 参数名称缩写

内敛闭包可缩写参数名称

```swift
reversedNames = names.sorted(by: { $0 > $1 } )
```

### 运算符方法

```swift
reversedNames = names.sorted(by: >)
```

## 尾随闭包

尾随闭包是一个书写在函数圆括号之后的闭包表达式，函数支持将其作为最后一个参数调用。在使用尾随闭包时，你不用写出它的参数标签：

```swift
func someFunctionThatTakesAClosure(closure: () -> Void) {
    // 函数体部分
}

// 以下是不使用尾随闭包进行函数调用
someFunctionThatTakesAClosure(closure: {
    // 闭包主体部分
})

// 以下是使用尾随闭包进行函数调用
someFunctionThatTakesAClosure() {
    // 闭包主体部分
}
```

```swift
let digitNames = [
    0: "Zero", 1: "One", 2: "Two",   3: "Three", 4: "Four",
    5: "Five", 6: "Six", 7: "Seven", 8: "Eight", 9: "Nine"
]
let numbers = [16, 58, 510]

let strings = numbers.map {
    (number) -> String in
    var number = number
    var output = ""
    repeat {
        output = digitNames[number % 10]! + output
        number /= 10
    } while number > 0
    return output
}
// strings 常量被推断为字符串类型数组，即 [String]
// 其值为 ["OneSix", "FiveEight", "FiveOneZero"]
```

## 值捕获

闭包可以在其被定义的上下文中**捕获**常量或变量。即使定义这些常量和变量的原作用域已经不存在，闭包仍然可以在闭包函数体内引用和修改这些值。


```swift
func makeIncrementer(forIncrement amount: Int) -> () -> Int {
    var runningTotal = 0
    func incrementer() -> Int {
        runningTotal += amount
        return runningTotal
    }
    return incrementer
}
```

## 闭包是引用类型

无论你将函数或闭包赋值给一个常量还是变量，你实际上都是将常量或变量的值设置为对应函数或闭包的引用

```swift
let alsoIncrementByTen = incrementByTen
alsoIncrementByTen()
```

## 逃逸闭包

当一个闭包作为参数传到一个函数中，但是这个闭包在函数返回之后才被执行，我们称该闭包从函数中逃逸。当你定义接受闭包作为参数的函数时，你可以在参数名之前标注 `@escaping`，用来指明这个闭包是允许“逃逸”出这个函数的。

```swift
var completionHandlers: [() -> Void] = []
func someFunctionWithEscapingClosure(completionHandler: @escaping () -> Void) {
    completionHandlers.append(completionHandler)
}
```


## 自动闭包

TODO: