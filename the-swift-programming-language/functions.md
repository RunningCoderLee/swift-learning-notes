# 函数

## 定义与调用

```swift
func greet(person: String) -> String {
    let greeting = "Hello, " + person + "!"
    return greeting
}

print(greet(person: "Anna"))
// 打印“Hello, Anna!”
print(greet(person: "Brian"))
// 打印“Hello, Brian!”
```

## 参数与返回值

### 无参数

```swift
func sayHelloWorld() -> String {
    return "hello, world"
}
print(sayHelloWorld())
// 打印“hello, world”
```

### 多参数

```swift
func greet(person: String, alreadyGreeted: Bool) -> String {
    if alreadyGreeted {
        return greetAgain(person: person)
    } else {
        return greet(person: person)
    }
}
print(greet(person: "Tim", alreadyGreeted: true))
// 打印“Hello again, Tim!”
```

### 无返回值

**注意：** 严格地说，即使没有明确定义返回值，该 `greet(Person：)` 函数仍然返回一个值。没有明确定义返回类型的函数会返回一个 `Void` 类型特殊值，该值为一个空元组，写成 `()`。

```swift
func greet(person: String) {
    print("Hello, \(person)!")
}
greet(person: "Dave")
// 打印“Hello, Dave!”
```

### 多重返回值

可以用`元组（tuple）`类型让多个值作为一个复合值从函数中返回。

```swift
func minMax(array: [Int]) -> (min: Int, max: Int) {
    var currentMin = array[0]
    var currentMax = array[0]
    for value in array[1..<array.count] {
        if value < currentMin {
            currentMin = value
        } else if value > currentMax {
            currentMax = value
        }
    }
    return (currentMin, currentMax)
}
```

### 可选元祖返回

如果函数返回的元组类型有可能整个元组都“没有值”，你可以使用`可选的`元组返回类型反映整个元组可以是 `nil` 的事实。你可以通过在元组类型的右括号后放置一个问号来定义一个可选元组，例如 `(Int, Int)?` 或 `(String, Int, Bool)?`

**注意：** 可选元组类型如 `(Int, Int)?` 与元组包含可选类型如 `(Int?, Int?)` 是不同的。可选的元组类型，整个元组是可选的，而不只是元组中的每个元素值。

```swift
func sayHelloWorld() -> String {
    return "hello, world"
}
print(sayHelloWorld())
// 打印“hello, world”
```

### 隐式返回

如果一个函数的整个函数体是一个单行表达式，这个函数可以隐式地返回这个表达式。

```swift
func greeting(person: String) -> String {
    "Hello, " + person + "!"
}
print(greeting(person: "Dave"))
// 打印 "Hello, Dave!"

func anotherGreeting(person: String) -> String {
    return "Hello, " + person + "!"
}
print(anotherGreeting(person: "Dave"))
// 打印 "Hello, Dave!"
```

## 参数标签和参数名称

每个函数参数都有一个`参数标签（argument label）`以及一个`参数名称（parameter name）`。`参数标签`在调用函数的时候使用；调用的时候需要将函数的`参数标签`写在对应的参数前面。`参数名称`在函数的实现中使用。默认情况下，函数参数使用`参数名称`来作为它们的`参数标签`。

### 指定参数标签

可以在参数名称前指定它的参数标签，中间以空格分隔（类似 别名的概念）：

```swift
func someFunction(argumentLabel parameterName: Int) {
    // 在函数体内，parameterName 代表参数值
}

func greet(person: String, from hometown: String) -> String {
    return "Hello \(person)!  Glad you could visit from \(hometown)."
}
print(greet(person: "Bill", from: "Cupertino"))
// 打印“Hello Bill!  Glad you could visit from Cupertino.”
```

### 忽略参数标签
如果你不希望为某个参数添加一个标签，可以使用一个下划线（`_`）来代替一个明确的参数标签。

```swift
func someFunction(_ firstParameterName: Int, secondParameterName: Int) {
     // 在函数体内，firstParameterName 和 secondParameterName 代表参数中的第一个和第二个参数值
}
someFunction(1, secondParameterName: 2)
```

### 默认参数值

你可以在函数体中通过给参数赋值来为任意一个参数定义`默认值（Deafult Value）`。当默认值被定义后，调用这个函数时可以忽略这个参数。

```swift
func someFunction(parameterWithoutDefault: Int, parameterWithDefault: Int = 12) {
    // 如果你在调用时候不传第二个参数，parameterWithDefault 会值为 12 传入到函数体中。
}
someFunction(parameterWithoutDefault: 3, parameterWithDefault: 6) // parameterWithDefault = 6
someFunction(parameterWithoutDefault: 4) // parameterWithDefault = 12
```

### 可变参数

一个`可变参数（variadic parameter）`可以接受零个或多个值。函数调用时，你可以用可变参数来指定函数参数可以被传入不确定数量的输入值。通过在变量类型名后面加入（`...`）的方式来定义可变参数。
可变参数的传入值在函数体中变为此类型的一个数组。例如，一个叫做 `numbers` 的 `Double... `型可变参数，在函数体内可以当做一个叫 `numbers` 的 `[Double]` 型的数组常量。

```swift
func arithmeticMean(_ numbers: Double...) -> Double {
    var total: Double = 0
    for number in numbers {
        total += number
    }
    return total / Double(numbers.count)
}
arithmeticMean(1, 2, 3, 4, 5)
// 返回 3.0, 是这 5 个数的平均数。
arithmeticMean(3, 8.25, 18.75)
// 返回 10.0, 是这 3 个数的平均数。
```

### 输入输出参数

如果想在函数内部修改传入的参数值，同时在函数调用结束后外部变量的值也发生改变，此时应该把这个参数定义为`输入输出参数（In-Out Parameters）`

定义时再参数定义前加 `inout` 关键字，在传入的时候需要再参数名前加 `&` 符号。

```swift
func swapTwoInts(_ a: inout Int, _ b: inout Int) {
    let temporaryA = a
    a = b
    b = temporaryA
}

var someInt = 3
var anotherInt = 107
swapTwoInts(&someInt, &anotherInt)
print("someInt is now \(someInt), and anotherInt is now \(anotherInt)")
// 打印“someInt is now 107, and anotherInt is now 3”
```


## 函数类型

每个函数都有种特定的函数类型，函数的类型由函数的参数类型和返回类型组成。

```swift
func addTwoInts(_ a: Int, _ b: Int) -> Int {
    return a + b
}
// 函数类型为 (Int, Int) -> Int

func printHelloWorld() {
    print("hello, world")
}
// 函数类型为 () -> void
```


### 使用函数类型
在 Swift 中，使用函数类型就像使用其他类型一样。例如，你可以定义一个类型为函数的常量或变量，并将适当的函数赋值给它：

```swift
var mathFunction: (Int, Int) -> Int = addTwoInts
```

### 函数类型作为参数类型

```swift
func printMathResult(_ mathFunction: (Int, Int) -> Int, _ a: Int, _ b: Int) {
    print("Result: \(mathFunction(a, b))")
}
printMathResult(addTwoInts, 3, 5)
// 打印“Result: 8”
```

### 函数类型作为返回类型

```swift
func stepForward(_ input: Int) -> Int {
    return input + 1
}
func stepBackward(_ input: Int) -> Int {
    return input - 1
}
```


## 嵌套函数

定义在全局中的函数叫 `全局函数（global functions）`。把函数定义在别的函数体中，称作 `嵌套函数（nested functions）`

```swift
func chooseStepFunction(backward: Bool) -> (Int) -> Int {
    func stepForward(input: Int) -> Int { return input + 1 }
    func stepBackward(input: Int) -> Int { return input - 1 }
    return backward ? stepBackward : stepForward
}
var currentValue = -4
let moveNearerToZero = chooseStepFunction(backward: currentValue > 0)
// moveNearerToZero now refers to the nested stepForward() function
while currentValue != 0 {
    print("\(currentValue)... ")
    currentValue = moveNearerToZero(currentValue)
}
print("zero!")
// -4...
// -3...
// -2...
// -1...
// zero!
```