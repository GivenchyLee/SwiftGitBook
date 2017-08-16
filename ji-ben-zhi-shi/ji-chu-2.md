#### 可选类型

在可能缺少值的情况下使用选项。可选表示两种可能性:要么有值，您可以打开可选的选项以访问该值，或者根本没有值。

> optionals的概念并不存在于C或objective - C中。
>
> 在objective - c中，最接近的东西是返回nil的能力，否则它将返回一个对象，而nil的意思是“没有有效对象”。
>
> 但是，这只适用于对象——它不适用于结构、基本C类型或枚举值。
>
> 对于这些类型，objective - c方法通常返回一个特殊值\(如NSNotFound\)来表示没有值。
>
> 这种方法假定方法的调用方知道有一个特殊的值来测试并记住检查它。
>
> Swift的optionals允许您在不需要特殊常量的情况下指示任何类型的值。

这里有一个例子，说明如何使用optionals来处理没有值的情况。

Swift的Int类型有一个初始化器，它尝试将字符串值转换为Int值。

然而，并不是每个字符串都可以转换成整数。

字符串“123”可以转换为数值123，但是字符串“hello,world”没有明显的数字值可以转换为。

下面的示例使用初始化器尝试将字符串转换为Int:

```swift
let possibleNumber = "123"

let convertedNumber = Int(possibleNumber)
// convertedNumber is inferred to be of type "Int?", or "optional Int"
```

由于初始化器可能失败，它返回一个可选的Int，而不是Int。可选的Int 写作 Int? 问号表示它所包含的值是可选的，这意味着它可能包含一些Int值，或者根本不包含任何值。

您将一个可选的变量设置为值为nil的值:

```swift
var serverResponseCode: Int? = 404
// serverResponseCode contains an actual Int value of 404

serverResponseCode = nil
// serverResponseCode now contains no value
```

> nil不能与非可选常数和变量一起使用。如果您的代码中的常量或变量需要在某些条件下不存在某个值，则始终将其声明为适当类型的可选值。

如果您定义了一个可选变量，而不提供默认值，则该变量将自动设置为nil:

```swift
var surveyAnswer: String?
// surveyAnswer is automatically set to nil
```

> Swift的nil不等于objective - c中nil。在objective - c中，nil是指向一个不存在对象的指针。在Swift中，nil不是一个pointer 它是没有值的某一类型。任何类型的Optionals都可以被设置为nil，而不仅仅是对象类型。

#### If语句和强制解包

您可以使用if语句来判断一个可选类型是否包含了一个值，它将可选选项与nil进行比较。用“等于”运算符\(= =\)或“不等于”运算符\(! =\)进行比较。

如果一个选项有一个值，它被认为是“不等于”nil:

```swift
if convertedNumber != nil {

    print("convertedNumber contains some integer value")

}
// Prints "convertedNumber contains some integer value."
```

一旦确定可选内容包含了一个值，您就可以通过添加一个感叹号\(!\)来访问它的底层值，并将其添加到可选名称的末尾。感叹号有效地说，“我知道这个选项肯定有一个值;请使用它。这被称为强制取消可选的值:

```swift
if convertedNumber != nil {

    print("convertedNumber has an integer value of \(convertedNumber!)")

}
// Prints "convertedNumber has an integer value of 123."
```

尝试使用!获取一个不存在的可选值会触发运行时错误。在使用!\(force-unwrap\)可选类型的值之前，一定要确保一个可选类型包含一个非空值。

#### 可选绑定

您使用可选绑定来确定一个可选是否包含一个值，如果是的话，将这个可用值作为一个临时常量或变量。可选绑定可以使用if和while语句来检查一个可选的内部值，并将该值提取到一个常量或变量中，作为单个操作的一部分。如果和while语句在控制流中更详细地描述。

为if语句编写一个可选绑定，如下:

```swift
if let constentName = someOptional {

    statements

}

//例如，你可以使用可选绑定而不是强制解包来重写possibleNumber示例

if let actualNumber = Int(possibleNumber) {

    print(" \"  \(possibleNumber)  \" has an integer value of \(actualNumber)")

} else {

    print("\"\(possibleNumber)\" could not be converted to an integer")

}
// Prints ""123" has an integer value of 123"
```

如果Int\(possibleNumber\)返回的是可选Int包含一个值，则为可选的值设置一个名为actualNumber的新常数。如果转换成功，则在If语句的第一个分支中actualNumber常量变成了可以使用的常量。它已经被初始化成了可选类型包含的值了，因此不需要使用!后缀来访问它的值。在这个示例中，actualNumber只是用来打印转换的结果。

您可以使用可选绑定的常量和变量。如果您想要在If语句的第一个分支中操作actualNumber的值，那么您可以编写If var actualNumber 来代替，而可选的值将作为变量提供，而不是一个常量。

您可以在单个if语句中包含任意多个可选绑定和布尔条件，以逗号分隔。如果可选绑定中的任何值为nil或任何布尔条件计算为false，那么整个If语句的条件被认为是假的。下列if语句是等价的:

```swift
//常规的if语句

if let firstNumber = Int("4") {

    if let secondNumber = Int("42") {

        if firstNumber < secondNumber && secondNumber < 100 {

            print("\(firstNumber) < \(secondNumber) < 100")

        }

    }

}

//底下是将这么多的条件写在了一行上面

if let firstNumber = Int("4"), let secondNumber = Int("42"), firstNumber < secondNumber && secondNumber < 100 {

    print("\(firstNumber < \(secondNumber) < 100")

}
// Prints "4 < 42 < 100"
```

#### 隐式解包可选

如上所述，optionals表示一个常量或变量被允许“没有值”。Optionals可以通过if语句进行检查，以查看是否存在一个值，并可以有条件地打开带有可选绑定的包，以访问可选的值，如果它确实存在的话。



有时候很明显从程序的结构,一个可选的总有一个值,该可选型被第一次赋值之后。在这些情况下,每次访问它可以除去需要检查和解包可选型的值,因为它可以安全地假定总是有一个值。



这样的可选类型被定义为隐式打开的optionals。您可以通过放置一个感叹号\(String!\)而不是一个问号\(String?\)来编写一个隐式的非包装选项。



隐式的非包装选项在可选的值在第一次定义之后立即存在并确定在以后的每一个点都存在时，都是可用的。在Swift中隐式打开的optionals的主要用途是在类初始化过程中。



隐式的非包装可选选项背后是一个正常的可选选项，但也可以被使用为非可选值，而不需要在每次访问时解包可选值。下面的示例显示了当它们作为一个显式字符串访问它们的包装值时，可选字符串和隐式未包装的可选字符串之间的行为差异:

```swift
let possibleString: String? = "An optional string"

let forcedString: String = possibleString! //requires an exclamation mark(叹号)

let assumedString: String! = "An implicitly unwrapped optional string"

let implicitString: String = assumedString //no need for an exclamation mark
```

您可以考虑一个隐式打开的可选选项，它允许在使用时自动打开。而不是在您每次使用隐式可选型变量名称后放置一个感叹号，您只需要在声明该选项的类型之后放置一个感叹号就好了。

您仍然可以像一般的可选选项那样对隐式未包装的可选项进行处理，以检查它是否包含一个值:

```swift
if assumedString != nil {

    print(assumedString)
}
// Prints "An implicitly unwrapped optional string."
```

您还可以使用可选绑定的隐式非包装选项来检查和打开单个语句中的值:

```swift
if let difiniteString = assumedString {
    
    print(definiteString)
    
}
// Prints "An implicitly unwrapped optional string."
```

> 当一个变量有可能在稍后变成nil时，不要使用隐式打开的可选选项。如果需要在变量的生命周期中检查nil值，则始终使用常规可选类型



