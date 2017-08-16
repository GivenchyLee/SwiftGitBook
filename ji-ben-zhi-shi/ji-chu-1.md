### 常量和变量

#### 常量和变量的声明

```swift
let maximumNumberOfLoginAttempts = 10

var currentLoginAttemp = 0
```

我们声明了一个常量，常量名字为 maximumNumberOfLoginAttempts, 于此同时我们将它的初值赋值成了10，同时我们也声明了一个名字为 currentLoginAttemp的变量，初值为0。

我们还可以定义多个常量或者变量在一行上面，变量或者常量之间用逗号隔开，如下。

```swift
var x = 0.0, y = 0.0, z = 0.0

let i = 1, j = 2, k = 3
```

> 在你的代码中如果有一个存储值的量是不需要改变的，那么我们建议你使用关键字let将其声明成constant常量，而需要在代码过程中改变的量，我们才才使用关键字var将其声明成variabale变量。

#### 显示的声明一个变量或者常量的类型

当你声明一个变量或者常量的时候，可以显示的提供该变量或者常量的类型，形式如下：

```swift
var welcomeMessage: String
```

上面这个例子，为一个名字为welcomeMessage的变量显示提供了一个String的类型解释，表明了这个变量可以存储任何String类型的值。代码中的冒号可以读作“...类型为...”，所以上面的代码完整的可以独坐，声明了一个名字为welcomeMessage类型为String类型的变量。

在以后的使用中，welcomeMessage可以被赋值成任意合法的字符串，例如下面：

```swift
var welcomeMessage = "Hello"
```

同样的你也可以在一行声明多个类型一样的变量或者常量，然后它们之间用逗号隔开，在最后一个变量或者常量的后面加上显示的类型解释，如下所示

```swift
var red, green, blue: Double
var zhangSan, liSi, wangMaZi: String
```

> 当你初始化一个变量或者常量时候，并且给予了这个常量或者变量的初始值，那么就可以省略显示的类型解释操作，因为系统可以通过你赋予变量的初始值自己推断出你声明的变量或者常量的类型的。如果没有提供初始值，系统就不能通过类型推断机制推断出我们声明的变量是什么类型，所以这个时候就需要我么自己显示的提供类型解释了。

#### 常量和变量的名字

swift中变量和常量的名字可以很随意，只要不是数字开头就行，甚至中文或者表情等也可以做常量或者变量名字

```swift
let π = 3.14159

let 你好 = "你好世界"

let 🐶🐮 = "dogcow"
```

> 如果你想使用swift的保留关键字来充当变量或者常量的名字的时候，需要用（）将保留关键字括起来，然而我们应该尽可能的避免使用系统保留的关键字来命名变量或者常量。

你可以改变一个已经存在了的变量的值为另外一个值只要它们的类型是互相兼容的，如下面例子所示，我们将friendlyWelcome的值从"Hello!"改变成了"Bonjour!"：

```swift
var friendlyWelcome = "Hello!"

friendlyWelcome = "Bonjour!"
```

不同于变量，对于一个常量来讲，一旦我们给它赋了初值，那么这个常量就不能再被赋予其他值了，如果我们尝试修改一个已经赋过初值的常量，系统会提示我们编译错误，表示常量的值不能修改的。

```swift
let languageName = "Swift"

languageName = "Swfit++"

//编译器会提示：This is a compile-time error: languageName cannot be changed.
```

Swift使用字符串插值将常量或变量的名称作为一个较长的字符串中的占位符，并迅速将其替换为该常量或变量的当前值。将名称用括号括起来，并在开口圆括号前添加上反斜杠。

```swift
print("The current value of friendlyWelcome is \(friendlyWelcome)")

// Prints "The current value of friendlyWelcome is Bonjour!"
```

#### 分号

不像其它语言，swift不需要在每一行的末尾添加分号，即使你添加了，编译器也不会报错。然而当你想在一行写多条程序语句的时候，分号是必须的。这个时候就需要用分号来分割这两个或者多个程序语句了。

```swfit
let cat = "tom"; print(cat)
```

#### 整数

swift提供了有8,16,32和64这4种有符号和无符号的表示方式，8位的整数对应的名字为Int8/UInt8；16位的整数对应的名字为Int16/UInt16；32位的整数对应的名字为Int32/UInt32；64位的整数对应的名字为Int64/UInt64

##### 整数的界

你可以得到每种类型整数的最大值和最小值通过它们的min和max属性：

```swfit
let minValue = UInt8.min 
// minValue is equal to 0, and is of type UInt8

let maxVlaue = UInt8.max 
// maxValue is equal to 255, and is of type UInt8
```

##### Int

在很多情况下，我们不需要很精确的确定整数的位数，这个时候我们可以用另外一个整数类型Int，在不同的平台上这个类型代表的整数长度也是不同的，在32位机器平台上Int和Int32的长度一样；在64位机器平台上时候Int和Int64的长度是一样的。除非你需要特定size的整数下写代码，如果不是那么需要的话，应该使用Int/UInt

#### 浮点数

swift提供了两种浮点数的类型，一种是Float（代表了32位长度的浮点数）另外一种是Double\(代表了64位长度的浮点类型）

#### 整数和浮点数之间的转换

```swift
let three = 3

let pointOneFourOneFiveNine = 0.14159

let pi = Double(three) + pointOneFourOneFiveNine 
// pi equals 3.14159, and is inferred to be of type Double
```

在这里，常量3的值用于创建Double类型的新值，因此添加的两边都是相同类型的。如果没有这种转换，加法是不允许的。浮点数到整型转换也必须是显式的。整数类型可以用Double或Float值初始化:

```swfit
let integerPi = Int(pi)
// integerPi equals 3, and is inferred to be of type Int
```

当使用这种方式初始化一个新的整数值时，浮点值总是被截断。这意味着4。75变成4，而- 3.9变成- 3。

#### 类型别名

类型别名为现有类型定义了一个替代名称。您可以使用typealias关键字定义类型别名。

```swift
typealias AudioSample = UInt16

var maxAmplitudeFound = AudioSample.min
// maxAmplitudeFound is now 0
```

#### Booleans

Swift有一个基本的布尔型，叫做Bool。布尔值被称为逻辑值，因为它们只能是真或假。Swift提供了两个布尔常数值，true和false:

```swift
let orangesAreOrange = true

let turnipsAreDelicious = false
```

布尔值在使用条件语句\(如if语句\)时特别有用:

```swift
if turnipsAreDelicious {

    print("Mmm, tasty turnips!")

} else {

    print("Eww, turnips are horrible.")

}
// Prints "Eww, turnips are horrible."
```

Swift的类型安全防止非布尔值被替换为Bool。以下示例报告编译时错误:

```swift
let i = 1

if i {
    // this example will not compile, and will report an error
}
```

然而，下面的替代示例是有效的

```swift
let i = 1

if i == 1 {
    // this example will compile successfully
}
```

#### 元组

```swift
let http404Error = (404, "Not Found")
// http404Error is of type (Int, String), and equals (404, "Not Found")

let (statusCode, statusMessage) = http404Error

print("This status code is \(statusCode)")
print("This status code is \(http404Error.0)")
// Prints "The status code is 404"

print("This status message is \(statusMassage)")
print("This status message is \(http404Error.1)")
// Prints "The status message is Not Found"

let(justTheStatusCode, _) = http404Error
prit("The status code is \(justTheStatusCode)")
// Prints "The status code is 404"



let http200Status = (statusCode: 200, description: "OK")
print("The status code is \(http200Status.statusCode)")
// Prints "The status code is 200"
print("The status message is \(http200Status.description)")
// Prints "The status message is OK"
```



