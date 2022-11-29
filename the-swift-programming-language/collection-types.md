# 集合类型

* 数组（Array）：有序数据的集合
* 集合（Set）：无序无重复数据的集合
* 字典（Dictionary）无序的键值对的集合


![]('./images/CollectionTypes_intro_2x.png')

## 可变性

* 分配给变量，集合可变
* 分配给常量，集合不可变

## 数组（Array）

数组使用有序列表存储同一类型的多个值。相同的值可以多次出现在一个数组的不同位置中。

完整写法：

* `Array<Element>`
* `[Element]`（推荐）

### 创建空数组

```swift
var someInts: [Int] = []
```

### 创建带有默认值的数组

```swift
var threeDoubles = Array(repeating: 0.0, count: 3)
// threeDoubles 是一种 [Double] 数组，等价于 [0.0, 0.0, 0.0]
```

### 通过相加创建一个数组

你可以使用加法操作符（`+`）来组合若干个**已存在**的**相同类型**数组。新数组的数据类型会从这几个数组的数据类型中推断出来：

```swift
var threeDoubles = Array(repeating: 0.0, count: 3)

var anotherThreeDoubles = Array(repeating: 2.5, count: 3)
// anotherThreeDoubles 被推断为 [Double]，等价于 [2.5, 2.5, 2.5]

var sixDoubles = threeDoubles + anotherThreeDoubles
// sixDoubles 被推断为 [Double]，等价于 [0.0, 0.0, 0.0, 2.5, 2.5, 2.5]

var nineDOubles = threeDoubles + anotherThreeDoubles + threeDoubles
// nineDOubles 被推断为 [Double], 等价于 [0, 0, 0, 2.5, 2.5, 2.5, 0, 0, 0]
```

### 用数组字面量构造数组

形式：`[value 1, value 2, value 3]`

```swift
var shoppingList: [String] = ["Eggs", "Milk"]
// shoppingList 已经被构造并且拥有两个初始项。

// 用字面量构造拥有相同类型值数组的时候，不必把数组的类型定义清楚，也可以这样写
var shoppingList = ["Eggs", "Milk"]
```

### 访问和修改数组

#### 访问

* 下标
* 方法或属性

```swift
var shoppingList = ["Eggs", "Milk"]

shoppingList[0] // "Eggs"
shoppingList.count // 2
shoppingList.isEmpty() // false
```

#### 修改

* 下标
* 方法或属性

```swift
var shoppingList = ["Eggs", "Milk"]

shoppingList.append("Flour") 
// shoppingList 现在有3个数据项

shoppingList[0] = "Six eggs"
// 其中的第一项现在是“Six eggs”而不是“Eggs”

// 使用加法赋值运算符（+=）直接将另一个相同类型数组中的数据添加到该数组后面：
shoppingList += ["Baking Powder"]
// shoppingList 现在有四项了
shoppingList += ["Chocolate Spread", "Cheese", "Butter"]
// shoppingList 现在有七项了

// 利用下标来一次改变一系列数据值
shoppingList[4...6] = ["Bananas", "Apples"]

shoppingList.insert("Maple Syrup", at: 0)
// shoppingList 现在有7项
// 现在是这个列表中的第一项是“Maple Syrup”

let mapleSyrup = shoppingList.remove(at: 0)
// 索引值为0的数据项被移除
// shoppingList 现在只有6项，而且不包括 Maple Syrup
// mapleSyrup 常量的值等于被移除数据项“Maple Syrup”
let apples = shoppingList.removeLast()
// 数组的最后一项被移除了
// shoppingList 现在只有5项，不包括 Apples
// apples 常量的值现在等于字符串“Apples”
```

### 数组遍历

使用 `for-in` 语法

```swift
for item in shoppingList {
    print(item)
}
// Six eggs
// Milk
// Flour
// Baking Powder
// Bananas

// 使用 enumerated 方法来获取数据项的值和索引值
for (index, value) in shoppingList.enumerated() {
    print("Item \(String(index + 1)): \(value)")
}
// Item 1: Six eggs
// Item 2: Milk
// Item 3: Flour
// Item 4: Baking Powder
// Item 5: Bananas
```

## 集合

存储相同类型并且没有确定顺序的值。当集合元素**顺序不重要**时或者希望**确保每个元素只出现一次**时可以使用集合而不是数组。

完整写法： `Set<Element>` `Element` 表示集合中允许存储的类型

### 创建和构造一个空的集合

```swift
var letters = Set<Character>()
print("letters is of type Set<Character> with \(letters.count) items.")
// 打印“letters is of type Set<Character> with 0 items.”

var letters: Set<Character> = []
```

### 用数组字面量创建集合

```swift
var favoriteGenres: Set<String> = ["Rock", "Classical", "Hip hop"]

var favoriteGenres: Set = ["Rock", "Classical", "Hip hop"]
```

### 访问和修改一个集合

只能通过集合得属性和方法来访问和修改

方式基本和数组类似

```swift
var favoriteGenres: Set = ["Rock", "Classical", "Hip hop"]

print("I have \(favoriteGenres.count) favorite music genres.")
// 打印“I have 3 favorite music genres.”

if favoriteGenres.isEmpty {
    print("As far as music goes, I'm not picky.")
} else {
    print("I have particular music preferences.")
}
// 打印“I have particular music preferences.”

favoriteGenres.insert("Jazz")
// favoriteGenres 现在包含4个元素

if let removedGenre = favoriteGenres.remove("Rock") {
    print("\(removedGenre)? I'm over it.")
} else {
    print("I never much cared for that.")
}
// 打印“Rock? I'm over it.”

if favoriteGenres.contains("Funk") {
    print("I get up on the good foot.")
} else {
    print("It's too funky in here.")
}
// 打印“It's too funky in here.”
```

### 遍历一个集合

使用 `for-in`

```swift
for genre in favoriteGenres {
    print("\(genre)")
}
// Classical
// Jazz
// Hip hop

// Swift 的 Set 类型没有确定的顺序，为了按照特定顺序来遍历一个集合中的值可以使用 sorted() 方法，它将返回一个有序数组，这个数组的元素排列顺序由操作符 < 对元素进行比较的结果来确定。

for genre in favoriteGenres.sorted() {
    print("\(genre)")
}
// Classical
// Hip hop
// Jazz
```

## 集合操作

### 基本操作

![]('./images/setVennDiagram_2x.png)

* 使用 `intersection(_:)` 方法根据两个集合的交集创建一个新的集合。
* 使用 `symmetricDifference(_:)` 方法根据两个集合不相交的值创建一个新的集合。
* 使用 `union(_:)` 方法根据两个集合的所有值创建一个新的集合。
* 使用 `subtracting(_:)` 方法根据不在另一个集合中的值创建一个新的集合。

```swift
let oddDigits: Set = [1, 3, 5, 7, 9]
let evenDigits: Set = [0, 2, 4, 6, 8]
let singleDigitPrimeNumbers: Set = [2, 3, 5, 7]

oddDigits.union(evenDigits).sorted()
// [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
oddDigits.intersection(evenDigits).sorted()
// []
oddDigits.subtracting(singleDigitPrimeNumbers).sorted()
// [1, 9]
oddDigits.symmetricDifference(singleDigitPrimeNumbers).sorted()
// [1, 2, 9]
```

### 集合成员关系和相等

下面的插图描述了三个集合 `a`、`b` 和 `c`，以及通过重叠区域表述集合间共享的元素。集合 `a` 是集合 `b` 的父集合，因为 `a` 包含了 `b` 中所有的元素。相反的，集合 `b` 是集合 `a` 的子集合，因为属于 `b` 的元素也被 `a` 包含。集合 `b` 和集合 `c` 是不相交的，因为它们之间没有共同的元素。

![]('./images/setEulerDiagram_2x.png)



* 使用 `==` 来判断两个集合包含的值是否全部相同。
* 使用 `isSubset(of:)` 方法来判断一个集合中的所有值是否也被包含在另外一个集合中。
* 使用 `isSuperset(of:)` 方法来判断一个集合是否包含另一个集合中所有的值。
* 使用 `isStrictSubset(of:)` 或者 `isStrictSuperset(of:)` 方法来判断一个集合是否是另外一个集合的子集合或者父集合并且两个集合并不相等。
* 使用 `isDisjoint(with:)` 方法来判断两个集合是否不含有相同的值（是否没有交集）。

```swift
let houseAnimals: Set = ["🐶", "🐱"]
let farmAnimals: Set = ["🐮", "🐔", "🐑", "🐶", "🐱"]
let cityAnimals: Set = ["🐦", "🐭"]

houseAnimals.isSubset(of: farmAnimals)
// true
farmAnimals.isSuperset(of: houseAnimals)
// true
farmAnimals.isDisjoint(with: cityAnimals)
// true
```

## 字典

字典是一种无序的集合，它存储的是键值对之间的关系，其所有键的值需要是相同的类型，所有值的类型也需要相同。

定义方法：

* `Dictionary<Key, Value>`
* `[Key: Value]`（推荐）

其中 `Key` 是一种可以在字典中被用作键的类型，`Value` 是字典中对应于这些键所存储值的数据类型。

### 创建空字典

```swift
var namesOfIntegers: [Int: String] = [:]
// namesOfIntegers 是一个空的 [Int: String] 字典
```

### 用字典字面量创建字典

```swift
var airports: [String: String] = ["YYZ": "Toronto Pearson", "DUB": "Dublin"]

// 也可通过 Swift 自行推断类型
var airports = ["YYZ": "Toronto Pearson", "DUB": "Dublin"]
```

### 访问和修改字典

* 下标
* 方法或属性
  
```swift
var airports = ["YYZ": "Toronto Pearson", "DUB": "Dublin"]

print("The dictionary of airports contains \(airports.count) items.")
// 打印“The dictionary of airports contains 2 items.”（这个字典有两个数据项）

if airports.isEmpty {
    print("The airports dictionary is empty.")
} else {
    print("The airports dictionary is not empty.")
}
// 打印“The airports dictionary is not empty.”

airports["LHR"] = "London"
// airports 字典现在有三个数据项

airports["LHR"] = "London Heathrow"
// “LHR”对应的值被改为“London Heathrow”

if let oldValue = airports.updateValue("Dublin Airport", forKey: "DUB") {
    print("The old value for DUB was \(oldValue).")
}
// 输出“The old value for DUB was Dublin.”

airports["APL"] = "Apple Internation"
// “Apple Internation”不是真的 APL 机场，删除它
airports["APL"] = nil
// APL 现在被移除了

if let removedValue = airports.removeValue(forKey: "DUB") {
    print("The removed airport's name is \(removedValue).")
} else {
    print("The airports dictionary does not contain a value for DUB.")
}
// 打印“The removed airport's name is Dublin Airport.”
```

### 字典遍历

使用 `for-in`，每一个字典中的数据项都以 `(key, value)` 元组形式返回，并且可以使用临时常量或者变量来分解这些元组

```swift
for (airportCode, airportName) in airports {
    print("\(airportCode): \(airportName)")
}
// YYZ: Toronto Pearson
// LHR: London Heathrow

for airportCode in airports.keys {
    print("Airport code: \(airportCode)")
}
// Airport code: YYZ
// Airport code: LHR

for airportName in airports.values {
    print("Airport name: \(airportName)")
}
// Airport name: Toronto Pearson
// Airport name: London Heathrow
```

如果你需要使用某个字典的键集合或者值集合来作为某个接受 Array 实例的 API 的参数，可以直接使用 keys 或者 values 属性构造一个新数组：

```swift
let airportCodes = [String](airports.keys)
// airportCodes 是 ["YYZ", "LHR"]

let airportNames = Array(airports.values)
// airportNames 是 ["Toronto Pearson", "London Heathrow"]
```