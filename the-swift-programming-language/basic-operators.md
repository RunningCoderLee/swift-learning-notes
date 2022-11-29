# 基本运算符

## 赋值运算符

用来初始化或更新，**不返回任何值**

```swift
let b = 10
var a = 5
a = b

// 如果赋值的右边是一个多元组，它的元素可以马上被分解成多个常量或变量：
let (x, y) = (1, 2)
```

## 算数运算符

* 加法（+）
* 减法（-）
* 乘法（*）
* 除法（/）

加法运算符也可用于 String 的拼接：
```swift
"hello, " + "world"  // 等于 "hello, world"`
```

## 求余运算符
求余运算符（`a % b`）是计算 `b` 的多少倍刚刚好可以容入 `a`，返回多出来的那部分（余数）。

```swift
9 % 4 = 1

-9 % 4  = -1
```

**注意：** 在对负数 b 求余时，b 的符号会被忽略。这意味着 a % b 和 a % -b 的结果是相同的。

## 一元负号运算符
数值的正负号可以使用前缀 `-`（即一元负号符）来切换

```swift
let three = 3
let minusThree = -three       // minusThree 等于 -3
let plusThree = -minusThree   // plusThree 等于 3, 或 "负负3"
```

## 一元正号运算符
一元正号符（`+`）不做任何改变地返回操作数的值

```swift
let minusSix = -6
let alsoMinusSix = +minusSix  // alsoMinusSix 等于 -6
```

## 组合赋值运算符

把其他运算符和赋值运算（`=`）组合的组合赋值运算符，**没有返回值**

```swift
var a = 1
a += 2
// a += 2 相当于 a = a + 2
// a 现在是 3
```

## 比较运算符

返回一个标识表达式是否成立的布尔值

* 等于（`a == b`）
* 不等于（`a != b`）
* 恒等 （`a === b`）
* 不恒等（`a !== b`）
* 大于（`a > b`）
* 小于（`a < b`）
* 大于等于（`a >= b`）
* 小于等于（`a <= b`）

```swift
1 == 1   // true, 因为 1 等于 1
2 != 1   // true, 因为 2 不等于 1
2 > 1    // true, 因为 2 大于 1
1 < 2    // true, 因为 1 小于2
1 >= 1   // true, 因为 1 大于等于 1
2 <= 1   // false, 因为 2 并不小于等于 1
```

### 元祖比较
#### 前提条件
两个元组的元素相同，且长度相同。

> **注意：** Swift 标准库只能比较七个以内元素的元组比较函数。如果你的元组元素超过七个时，你需要自己实现比较运算符

#### 比较规则
比较元组大小会按照从左到右、逐值比较的方式，直到发现有两个值不等时开始进行比较。如果所有的值都相等，那么这一对元组我们就称它们是相等的。

```swift
(1, "zebra") < (2, "apple")   // true，因为 1 小于 2
(1, 2, "zebra") < (1, 3, "apple")   // true，因为 2 小于 3
(3, "apple") < (3, "bird")    // true，因为 3 等于 3，但是 apple 小于 bird
(4, "dog") == (4, "dog")      // true，因为 4 等于 4，dog 等于 dog
```
### 三元运算符

形式：`问题 ? 答案 1 : 答案 2`

```swift
if question {
	answer1
} else {
	answer2
}

// 等同于
question ? answer1 : answer2
```

## 空合运算符
形式： `a ?? b`

对可选类型 a 进行空判断，如果 a 包含一个值就进行解包，否则就返回一个默认值 b

等同于

```swift
a != nil ? a! : b
```

> **注意：**  表达式 `a` 必须是 `Optional` 类型。默认值 `b` 的类型必须要和 `a` 存储值的类型保持一致

## 区间运算符
### 闭区间运算符

闭区间运算符（`a...b`）定义一个包含从 `a` 到 `b`（包括 `a` 和 `b`）的所有值的区间。`a` 的值不能超过 `b`。

```swift
for index in 1...5 {
	print("\(index) * 5 = \(index * 5)")
}
// 1 * 5 = 5
// 2 * 5 = 10
// 3 * 5 = 15
// 4 * 5 = 20
// 5 * 5 = 25
```

### 半开区间运算符

半开区间运算符（`a..<b`）定义一个从 `a` 到 `b` 但不包括 `b` 的区间。

```swift
let names = ["Anna", "Alex", "Brian", "Jack"]
let count = names.count
for i in 0..<count {
	print("第 \(i + 1) 个人叫 \(names[i])")
}
// 第 1 个人叫 Anna
// 第 2 个人叫 Alex
// 第 3 个人叫 Brian
// 第 4 个人叫 Jack
```

### 单侧区间
表示往一侧无限延伸的区间

#### 单侧闭区间

```swift
let names = ["Anna", "Alex", "Brian", "Jack"]

for name in names[2...] {
    print(name)
}
// Brian
// Jack

for name in names[...2] {
    print(name)
}
// Anna
// Alex
// Brian
```

#### 单侧开区间

```swift
let names = ["Anna", "Alex", "Brian", "Jack"]

for name in names[..<2] {
    print(name)
}
// Anna
// Alex
```

## 逻辑运算符

* 逻辑非（!a）
* 逻辑与（a && b）
* 逻辑或（a || b）

**可以组合**
```swift
if enteredDoorCode && passedRetinaScan || hasDoorKey || knowsOverridePassword {
	print("Welcome!")
} else {
	print("ACCESS DENIED")
}
```

**可以通过括号来明确优先级**
```swift
if (enteredDoorCode && passedRetinaScan) || hasDoorKey || knowsOverridePassword {
	print("Welcome!")
} else {
	print("ACCESS DENIED")
}
```
