# 控制流

## For-In

用来遍历集合中的所有元素，遍历元素时的顺序是**无法确定**的

```swift
// 数组
let names = ["Anna", "Alex", "Brian", "Jack"]
for name in names {
    print("Hello, \(name)!")
}
// Hello, Anna!
// Hello, Alex!
// Hello, Brian!
// Hello, Jack!

// 字典
let numberOfLegs = ["spider": 8, "ant": 6, "cat": 4]
for (animalName, legCount) in numberOfLegs {
    print("\(animalName)s have \(legCount) legs")
}
// cats have 4 legs
// ants have 6 legs
// spiders have 8 legs

// 通过区间运算符指定范围
for index in 1...5 {
    print("\(index) times 5 is \(index * 5)")
}
// 1 times 5 is 5
// 2 times 5 is 10
// 3 times 5 is 15
// 4 times 5 is 20
// 5 times 5 is 25

// 忽略 index
let base = 3
let power = 10
var answer = 1
for _ in 1...power {
    answer *= base
}
print("\(base) to the power of \(power) is \(answer)")
// 输出“3 to the power of 10 is 59049”
```

## While

`while` 循环从计算一个条件开始。如果条件为 `true`，会重复运行一段语句，直到条件变为 `false`。

```swift
while condition {
	statements
}
```


## Repeat-While

`while` 循环的另外一种形式是 `repeat-while`，它和 `while` 的区别是在判断循环条件之前，先执行一次循环的代码块。然后重复循环直到条件为 `false`。类似其他语言的 `do-while` 

```swift
repeat {
	statements
} while condition
```

## 条件语句

### If

只有条件为 `true` 时，才执行相关代码

```swift
var temperatureInFahrenheit = 30
if temperatureInFahrenheit <= 32 {
    print("It's very cold. Consider wearing a scarf.")
} else if temperatureInFahrenheit >= 86 {
    print("It's really warm. Don't forget to wear sunscreen.")
} else {
    print("It's not that cold. Wear a t-shirt.")
}
```

### Switch

`switch` 语句会尝试把某个值与若干个模式（`pattern`）进行匹配。根据第一个匹配成功的模式，switch 语句会执行对应的代码。当有可能的情况较多时，通常用 `switch` 语句替换 if 语句。

**注意：** 
* 不存在隐式的贯穿，即当匹配到 `case` 分支条件并执行完分支中的代码后，不会继续执行下一个 `case` 分支，因此 `case` 分支中无需显示使用 `break` 语句。（依然可以在代码执行完毕前通过 `break` 跳出）
* 每一个 case 分支都必须包含至少一条语句
* 可以通过逗号将多个值合并成一个复核匹配

```swift
switch some value to consider {
case value 1:
    respond to value 1
case value 2,
    value 3:
    respond to value 2 or 3
default:
    otherwise, do something else
}
```

#### 区间匹配
let approximateCount = 62
let countedThings = "moons orbiting Saturn"
let naturalCount: String
switch approximateCount {
case 0:
    naturalCount = "no"
case 1..<5:
    naturalCount = "a few"
case 5..<12:
    naturalCount = "several"
case 12..<100:
    naturalCount = "dozens of"
case 100..<1000:
    naturalCount = "hundreds of"
default:
    naturalCount = "many"
}
print("There are \(naturalCount) \(countedThings).")
// 输出“There are dozens of moons orbiting Saturn.”
```swift

```

#### 元祖

我们可以使用元组在同一个 `switch` 语句中测试多个值。元组中的元素可以是**值**，也可以是**区间**。另外，使用下划线（`_`）来匹配所有可能的值。

```swift
let somePoint = (1, 1)
switch somePoint {
case (0, 0):
    print("\(somePoint) is at the origin")
case (_, 0):
    print("\(somePoint) is on the x-axis")
case (0, _):
    print("\(somePoint) is on the y-axis")
case (-2...2, -2...2):
    print("\(somePoint) is inside the box")
default:
    print("\(somePoint) is outside of the box")
}
// 输出“(1, 1) is inside the box”
```

#### 值绑定

case 分支允许将匹配的值声明为临时常量或变量，并且在 case 分支体内使用 —— 这种行为被称为值绑定（value binding），因为匹配的值在 case 分支体内，与临时的常量或变量绑定。

```swift
let anotherPoint = (2, 0)
switch anotherPoint {
case (let x, 0):
    print("on the x-axis with an x value of \(x)")
case (0, let y):
    print("on the y-axis with a y value of \(y)")
case let (x, y):
    print("somewhere else at (\(x), \(y))")
}
// 输出“on the x-axis with an x value of 2”
```

#### Where

case 分支的模式可以使用 `where` 语句来判断额外的条件。

```swift
let yetAnotherPoint = (1, -1)
switch yetAnotherPoint {
case let (x, y) where x == y:
    print("(\(x), \(y)) is on the line x == y")
case let (x, y) where x == -y:
    print("(\(x), \(y)) is on the line x == -y")
case let (x, y):
    print("(\(x), \(y)) is just some arbitrary point")
}
// 输出“(1, -1) is on the line x == -y”****
```

#### 复合型 Cases

当多个条件可以使用同一种方法来处理时，可以将这几种可能放在同一个 `case` 后面，并且用逗号隔开。当 `case` 后面的任意一种模式匹配的时候，这条分支就会被匹配。并且，如果匹配列表过长，还可以分行书写：

```swift
let someCharacter: Character = "e"
switch someCharacter {
case "a", "e", "i", "o", "u":
    print("\(someCharacter) is a vowel")
case "b", "c", "d", "f", "g", "h", "j", "k", "l", "m",
     "n", "p", "q", "r", "s", "t", "v", "w", "x", "y", "z":
    print("\(someCharacter) is a consonant")
default:
    print("\(someCharacter) is not a vowel or a consonant")
}
// 输出“e is a vowel”
```

## 控制转移语句

### Continue

`continue` 语句告诉一个循环体立刻停止本次循环，重新开始下次循环。就好像在说“本次循环我已经执行完了”，但是并不会离开整个循环体。

```swift
let puzzleInput = "great minds think alike"
var puzzleOutput = ""
for character in puzzleInput {
    switch character {
    case "a", "e", "i", "o", "u", " ":
        continue
    default:
        puzzleOutput.append(character)
    }
}
print(puzzleOutput)
    // 输出“grtmndsthnklk”
```

### Break
`break` `语句会立刻结束整个控制流的执行。break` 可以在 `switch` 或循环语句中使用，用来提前结束 `switch` 或循环语句。


#### 循环语句中的 `break`
当在一个循环体中使用 `break` 时，会立刻中断该循环体的执行，然后跳转到表示循环体结束的大括号（`}`）后的第一行代码。不会再有本次循环的代码被执行，也不会再有下次的循环产生。

```swift
for index in 1...5 {
    if index > 3 {
        break
    }
    
    print(index)
}
// 1
// 2
// 3
```

#### `switch` 语句中的 `break`

当在一个 `switch` 代码块中使用 `break` 时，会立即中断该 `switch` 代码块的执行，并且跳转到表示 `switch` 代码块结束的大括号（`}`）后的第一行代码。

### fallthrough

如果需要 `switch` 语句支持贯穿特性，在需要该特性的 `case` 分支中使用 `fallthrough`

```swift
let integerToDescribe = 5
var description = "The number \(integerToDescribe) is"
switch integerToDescribe {
case 2, 3, 5, 7, 11, 13, 17, 19:
    description += " a prime number, and also"
    fallthrough
default:
    description += " an integer."
}
print(description)
// 输出“The number 5 is a prime number, and also an integer.”
```
### 带标签的语句

在多个循环体或者条件语句中嵌套中可以通过对循环体或条件语句使用标签来标记，同时显示指明 `break` 或 `continue` 语句生效的范围

```swift
labelName: while condition {
    statements
}
```

## 提前退出

像 `if` 语句一样，`guard` 的执行取决于一个表达式的布尔值。我们可以使用 `guard` 语句来要求条件必须为真时，以执行 `guard` 语句后的代码。不同于 `if` 语句，一个 `guard` 语句总是有一个 `else` 从句，如果条件不为真则执行 `else` 从句中的代码。

如果 `guard` 语句的条件被满足，则继续执行 `guard` 语句大括号后的代码。将变量或者常量的可选绑定作为 `guard` 语句的条件，都可以保护 guard 语句后面的代码。

```swift
func greet(person: [String: String]) {
    guard let name = person["name"] else {
        return
    }

    print("Hello \(name)!")

    guard let location = person["location"] else {
        print("I hope the weather is nice near you.")
        return
    }

    print("I hope the weather is nice in \(location).")
}

greet(person: ["name": "John"])
// 输出“Hello John!”
// 输出“I hope the weather is nice near you.”
greet(person: ["name": "Jane", "location": "Cupertino"])
// 输出“Hello Jane!”
// 输出“I hope the weather is nice in Cupertino.”
```

## 检测 API 可用性

在 `if` 或 `guard` 中使用 `可用性条件`去有条件的执行一段代码

* `#available(平台名称 版本号, ..., *)`
* `@unavailable(平台名称 版本号)`
* `@avaliable(平台名称 版本号, ..., *)`