# Golang学习笔记



## 基础

### Golang的返回值

在 Go 语言中，函数可以有多个返回值。返回值可以是任意类型，甚至可以是函数类型。以下是几种常见的函数返回值的写法：

单个返回值：一个函数可以返回单个值。

```go
func add(a, b int) int {
    return a + b
}
```

多个返回值：一个函数可以返回多个值。

```go
func divide(a, b int) (int, error) {
    if b == 0 {
        return 0, errors.New("division by zero")
    }
    return a / b, nil
}
```

```go
func test() (a, b, c int, result int) {
    a = 1
    b = 2
    c = 3
    return a, b, c, a + b + c
}

func main() {
    _, _, _, result := test()
    fmt.Println(result)
}
```



在 Go 语言中，`:=` 和 `=` 是两种不同的赋值操作符。下面是它们之间的区别：

1. `:=`：这是一个短变量声明操作符，用于声明并初始化一个变量。当使用 `:=` 进行赋值时，Go 会自动推断变量的类型。例如，`z := x + y` 表示声明一个新变量 `z` 并将 `x + y` 的结果赋给它。
2. `=`：这是普通的赋值操作符，用于将一个值赋给一个已经声明过的变量。在 `z = x + y` 中，`z` 已经在之前的代码中声明过，现在它被赋值为 `x + y` 的结果。

总的来说，`:=` 主要用于变量的声明和初始化，而 `=` 则用于普通的赋值操作。在您的例子中，如果 `z` 已经在之前的代码中声明过，并且您只是想将 `x + y` 的结果赋给它，应该使用 `=` 进行赋值。如果您希望声明一个新的变量并将 `x + y` 的结果赋给它，可以使用 `:=`。

### 匿名函数

匿名函数是指不需要定义函数名的一种函数实现方式。1958年LISP首先采用匿名函数。

在Go里面，函数可以像普通变量一样被传递或使用，Go语言支持随时在代码里定义匿名函数。

匿名函数由一个不带函数名的函数声明和函数体组成。匿名函数的优越性在于可以直接使用函数内的变量，不必申明。

```go
package main

import (
    "fmt"
    "math"
)

func main() {
    getSqrt := func(a float64) float64 {
        return math.Sqrt(a)
    }
    fmt.Println(getSqrt(4))
}
```

### if判断

if判断可以包含一个初始化语句，如下：

```go
 x := 0

// if x > 10        // Error: missing condition in if statement
// {
// }

if n := "abc"; x > 0 {     // 初始化语句未必就是定义变量， 如 println("init") 也是可以的。
    println(n[2])
} else if x < 0 {    // 注意 else if 和 else 左大括号位置。
    println(n[1])
} else {
    println(n[0])
}
```

### 闭包的使用

```go
package main

import "fmt"

var a = 20

func close() func() int {
	var x int = 10
	return func() int {
		return x
	}
}

func main() {
	resultFunc := close()
	result := resultFunc() + a
	fmt.Println(result)
}
```

### 嵌套判断语句

```go
package main

import "fmt"

func main() {
   /* 定义局部变量 */
   var a int = 100
   var b int = 200
   /* 判断条件 */
   if a == 100 {
       /* if 条件语句为 true 执行 */
       if b == 200 {
          /* if 条件语句为 true 执行 */
          fmt.Printf("a 的值为 100 ， b 的值为 200\n" )
       }
   }
   fmt.Printf("a 值为 : %d\n", a )
   fmt.Printf("b 值为 : %d\n", b )
}
```

### swtich语句

```go
package main

import "fmt"

func main() {
    fruit := "apple"

    switch fruit {
    case "apple":
        fmt.Println("It's an apple")
    case "banana":
        fmt.Println("It's a banana")
    case "orange":
        fmt.Println("It's an orange")
    default:
        fmt.Println("Unknown fruit")
    }
}
```

### for循环

```go
s := "abc"

for i, n := 0, len(s); i < n; i++ { // 常见的 for 循环，支持初始化语句。
    println(s[i])
}

n := len(s)
for n > 0 {                // 替代 while (n > 0) {}
    println(s[n])        // 替代 for (; n > 0;) {}
    n-- 
}
```

上面代码只能输出asicc码，下面代码才能输出字符串

```go
import "fmt"

func test1() {

	str := "hello"
	for i, c := range str {
		fmt.Printf("第 %d 位 的值 = %c\n", i, c)
	}
}
func main() {
	test1()
}
```

```go
func string_example(s string) {
	// 转换为小写
	fmt.Println(strings.ToLower(s))
	fmt.Println(strings.ToUpper(s))

	// 剩下字母和数字
	var cleanedStr strings.Builder
	for _, r := range s {
		if unicode.IsLetter(r) || unicode.IsDigit(r) {
			cleanedStr.WriteRune(r)
		}
	}
	fmt.Println(cleanedStr.String())
}
```



### 循环语句range

range语句用于迭代

```go
package main

func main() {
	s := "abc"
	// 忽略 2nd value，支持 string/array/slice/map。
	for i := range s {
		println(string(s[i]))
	}
	mydict := map[string]int{
		"a": 1,
		"b": 2,
		"c": 3,
	}
	for k, v := range mydict {
		println(k, v)
	}
	for _, c := range s {
		println(c)
	}
}

```


for可以

遍历array和slice

遍历key为整型递增的map

遍历string

for range可以完成所有for可以做的事情，却能做到for不能做的，包括

遍历key为string类型的map并同时获取key和value

遍历channel

### map类型，即字典

在 Go 语言中，`map` 类型用于存储键值对数据，类似于其他编程语言中的字典或关联数组。每个键值对在 `map` 中都必须唯一，并且可以通过键来快速查找对应的值。

您可以通过 `make()` 函数创建一个空的 `map`，然后通过添加键值对来向其中存储数据。`map` 的键可以是任何相等性操作符支持的类型，通常是基本数据类型或结构体类型。

以下是一个简单的示例，展示如何创建和使用 `map`：

```go
package main

import "fmt"

func main() {
	// 创建一个空的map
	m := make(map[string]int)

	// 向map中添加键值对
	m["apple"] = 10
	m["banana"] = 5

	// 通过键来获取值
	fmt.Println("apple 的数量:", m["apple"])
	fmt.Println("banana 的数量:", m["banana"])

	// 遍历map
	for key, value := range m {
		fmt.Println(key, ":", value)
	}
}

```

### 判断某个键是否存在

```go
func main() {
    scoreMap := make(map[string]int)
    scoreMap["张三"] = 90
    scoreMap["小明"] = 100
    // 如果key存在ok为true,v为对应的值；不存在ok为false,v为值类型的零值
    v, ok := scoreMap["张三"]
    if ok {
        fmt.Println(v)
    } else {
        fmt.Println("查无此人")
    }
}
```

### 只遍历键

```go
func main() {
    scoreMap := make(map[string]int)
    scoreMap["张三"] = 90
    scoreMap["小明"] = 100
    scoreMap["王五"] = 60
    for k := range scoreMap {
        fmt.Println(k)
    }
}
```

### 字符串

```go
package main

import (
    "fmt"
    "strings"
)

func main() {
    // 获取字符串长度
    str := "Hello, World!"
    fmt.Println("字符串长度:", len(str))

    // 字符串拼接
    str1 := "Hello"
    str2 := "World"
    result := str1 + ", " + str2 + "!"
    fmt.Println("拼接后的字符串:", result)

    // 字符串切割
    sentence := "The quick brown fox jumps over the lazy dog"
    words := strings.Split(sentence, " ")
    fmt.Println("切割后的单词:")
    for _, word := range words {
        fmt.Println(word)
    }

    // 字符串查找
    position := strings.Index(sentence, "fox")
    fmt.Println("字符串 'fox' 在句子中的位置:", position)

    // 字符串替换
    newSentence := strings.Replace(sentence, "fox", "cat", -1)
    fmt.Println("替换 'fox' 后的句子:", newSentence)
}
```

### unicode

Go中的unicode包提供了用于处理Unicode码点(code points)和UTF-8编码的函数和常量。

在Go语言中,字符串是用UTF-8编码表示的,而rune是Go语言中用于表示Unicode码点的数据类型。一个rune实际上是一个int32类型的值,可以存储任何有效的Unicode码点值。

unicode包中提供了一些常用的函数,例如:

1. `unicode.IsLetter(r rune) bool`
   - 判断给定的rune是否是字母字符。
2. `unicode.IsDigit(r rune) bool`
   - 判断给定的rune是否是数字字符。
3. `unicode.IsSpace(r rune) bool`
   - 判断给定的rune是否是空白字符。
4. `unicode.ToLower(r rune) rune`
   - 将给定的rune转换为小写形式。
5. `unicode.ToUpper(r rune) rune`
   - 将给定的rune转换为大写形式。

### 数组的初始化

```go
package main

import (
    "fmt"
)

var arr0 [5]int = [5]int{1, 2, 3}
var arr1 = [5]int{1, 2, 3, 4, 5}
var arr2 = [...]int{1, 2, 3, 4, 5, 6}
var str = [5]string{3: "hello world", 4: "tom"}

func main() {
    a := [3]int{1, 2}           // 未初始化元素值为 0。
    b := [...]int{1, 2, 3, 4}   // 通过初始化值确定数组长度。
    c := [5]int{2: 100, 4: 200} // 使用引号初始化元素。
    d := [...]struct {
        name string
        age  uint8
    }{
        {"user1", 10}, // 可省略元素类型。
        {"user2", 20}, // 别忘了最后一行的逗号。
    }
    fmt.Println(arr0, arr1, arr2, str)
    fmt.Println(a, b, c, d)
}
```

> var写在函数之外，用于定义全局变量

在 Go 语言中，如果在声明数组时没有初始化所有元素，未初始化的元素将会被赋予其类型的零值。

对于字符串数组 `var str = [5]string{3: "hello world", 4: "tom"}`，其中包含了两个初始化的元素：索引为 3 的元素为 "hello world"，索引为 4 的元素为 "tom"。而对于索引为 0、1 和 2 的元素，它们没有被初始化，因此它们会被赋予字符串的零值，即空字符串 ""。

### 数组

求数组的所有元素之和

```go
package main

import (
    "fmt"
    "math/rand"
    "time"
)

// 求元素和
func sumArr(a [10]int) int {
    var sum int = 0
    for i := 0; i < len(a); i++ {
        sum += a[i]
    }
    return sum
}

func main() {
    // 若想做一个真正的随机数，要种子
    // seed()种子默认是1
    //rand.Seed(1)
    rand.Seed(time.Now().Unix())

    var b [10]int
    for i := 0; i < len(b); i++ {
        // 产生一个0到1000随机数
        b[i] = rand.Intn(1000)
    }
    sum := sumArr(b)
    fmt.Printf("sum=%d\n", sum)
}
```

### 大小可调整的数组就是切片

在 Go 语言中，可以将一个数组赋值给一个切片，可以通过以下两种方式实现：

1. 使用切片表达式：

   ```go
   arr := [5]int{1, 2, 3, 4, 5}
   slice := arr[:]
   ```

   上述代码中，首先声明了一个长度为 5 的数组 `arr`，然后通过切片表达式 `arr[:]` 将数组 `arr` 赋值给了切片 `slice`。

2. 使用 `make()` 函数：

   ```go
   arr := [5]int{1, 2, 3, 4, 5}
   slice := make([]int, len(arr))
   copy(slice, arr[:])
   ```

   在这种方式中，首先声明了一个长度为 5 的数组 `arr`，然后使用 `make()` 函数创建一个切片 `slice`，并指定切片的长度为数组的长度。接着使用 `copy()` 函数将数组 `arr` 中的元素复制到了切片 `slice` 中。

### 切片大小

```go
package main

import (
    "fmt"
)

func main() {
    var a = []int{1, 3, 4, 5}
    fmt.Printf("slice a : %v , len(a) : %v\n", a, len(a))
    b := a[1:2]
    fmt.Printf("slice b : %v , len(b) : %v\n", b, len(b))
    c := b[0:3]
    fmt.Printf("slice c : %v , len(c) : %v\n", c, len(c))
}
```

### 字符串也是一种切片

```go
package main

import (
    "fmt"
)

func main() {
    str := "Hello world"
    s := []byte(str) //中文字符需要用[]rune(str)
    s[6] = 'G'
    s = s[:8]
    s = append(s, '!')
    str = string(s)
    fmt.Println(str)
}
```

## 内置函数

### time

获取时间

```go
func timeDemo() {
    now := time.Now() //获取当前时间
    fmt.Printf("current time:%v\n", now)

    year := now.Year()     //年
    month := now.Month()   //月
    day := now.Day()       //日
    hour := now.Hour()     //小时
    minute := now.Minute() //分钟
    second := now.Second() //秒
    fmt.Printf("%d-%02d-%02d %02d:%02d:%02d\n", year, month, day, hour, minute, second)
}
```

时间戳

时间戳是自1970年1月1日（08:00:00GMT）至当前时间的总毫秒数。它也被称为Unix时间戳（UnixTimestamp）。

```go
func timestampDemo() {
    now := time.Now()            //获取当前时间
    timestamp1 := now.Unix()     //时间戳
    timestamp2 := now.UnixNano() //纳秒时间戳
    fmt.Printf("current timestamp1:%v\n", timestamp1)
    fmt.Printf("current timestamp2:%v\n", timestamp2)
}
```



时间加

```go
func main() {
    now := time.Now()
    later := now.Add(time.Hour) // 当前时间加1小时后的时间
    fmt.Println(later)
}
```

定时器

```go
func tickDemo() {
    ticker := time.Tick(time.Second) //定义一个1秒间隔的定时器
    for i := range ticker {
        fmt.Println(i)//每秒都会执行的任务
    }
}
```

格式化

```go
func formatDemo() {
    now := time.Now()
    // 格式化的模板为Go的出生时间2006年1月2号15点04分 Mon Jan
    // 24小时制
    fmt.Println(now.Format("2006-01-02 15:04:05.000 Mon Jan"))
    // 12小时制
    fmt.Println(now.Format("2006-01-02 03:04:05.000 PM Mon Jan"))
    fmt.Println(now.Format("2006/01/02 15:04"))
    fmt.Println(now.Format("15:04 2006/01/02"))
    fmt.Println(now.Format("2006/01/02"))
}
```



### strconv

- `strconv.Itoa` 用于将整数转换为对应的十进制字符串表示形式。

- `strconv.Atoi` 用于将字符串转换为整数。
- `strconv.ParseFloat` 用于将字符串转换为浮点数。
- `strconv.FormatFloat` 用于将浮点数转换为字符串。

```go
package main

import (
	"fmt"
	"strconv"
)

func main() {
	// Using strconv.Itoa to convert an integer to a string
	num := 42
	strNum := strconv.Itoa(num)
	fmt.Printf("Integer %d converted to string: %s\n", num, strNum)

	// Using strconv.Atoi to convert a string to an integer
	str := "123"
	intNum, err := strconv.Atoi(str)
	if err != nil {
		fmt.Println("Error converting string to integer:", err)
	} else {
		fmt.Printf("String %s converted to integer: %d\n", str, intNum)
	}

	// Other strconv functions for conversions
	floatStr := "3.14"
	floatNum, err := strconv.ParseFloat(floatStr, 64)
	if err != nil {
		fmt.Println("Error converting string to float:", err)
	} else {
		fmt.Printf("String %s converted to float: %f\n", floatStr, floatNum)
	}

	// Formatting a float to a string
	f := 2.71828
	floatStr = strconv.FormatFloat(f, 'f', -1, 64)
	fmt.Printf("Float %f formatted to string: %s\n", f, floatStr)
}
```



## 指针

取地址操作符&和取值操作符`*`是一对互补操作符，`&`取出地址，`*`根据地址取出地址指向的值。

变量、指针地址、指针变量、取地址、取值的相互关系和特性如下：\

```
1.对变量进行取地址（&）操作，可以获得这个变量的指针变量。
2.指针变量的值是指针地址。
3.对指针变量进行取值（*）操作，可以获得指针变量指向的原变量的值。
```

示例

```go
package main

import "fmt"

func main() {
	//指针取值
	a := 10
	b := &a // 取变量a的地址，将指针保存到b中
	fmt.Printf("type of b:%T\n", b)
	c := *b // 指针取值（根据指针去内存取值）
	fmt.Printf("type of c:%T\n", c)
	fmt.Printf("value of c:%v\n", c)
}
```


再例

```go
func modify1(x int) {
    x = 100
}

func modify2(x *int) {
    *x = 100
}

func main() {
    a := 10
    modify1(a)
    fmt.Println(a) // 10
    modify2(&a)
    fmt.Println(a) // 100
}
```

### 指针变量

指针变量是一种特殊类型的变量，它存储的是内存地址。换句话说，指针变量指向内存中的一个地址，而不是直接存储数值或数据。使用指针变量可以在程序中间接地访问和修改变量的值，而不是直接操作变量本身。

下面声明了一个指针变量p，获取name字符串的地址

```go
package main

import "fmt"

var p *string

func main() {
	var name string = "John"

	p = &name
	fmt.Println(p)
	fmt.Println(*p)
}
```

```go
package main

import "fmt"

func main() {
	var a int
	fmt.Println(a)
	fmt.Println(&a)
	var p *int = &a
	// p = &a
	*p = 20
	fmt.Println(a)
}

```

a的值默认是0

## 结构体

Go语言中的基础数据类型可以表示一些事物的基本属性，但是当我们想表达一个事物的全部或部分属性时，这时候再用单一的基本数据类型明显就无法满足需求了，Go语言提供了一种自定义数据类型，可以封装多个基本数据类型，这种数据类型叫结构体，英文名称struct。 也就是我们可以通过struct来定义自己的类型了。

```go
 type person struct {
        name string
        city string
        age  int8
    }
```

上面是person的自定义类型，它有name、city、age三个字段，分别表示姓名、城市和年龄。这样我们使用这个person结构体就能够很方便的在程序中表示和存储人信息了。

### 匿名结构体

匿名结构体是一种没有显式命名的结构体类型。它们通常用于临时创建一个结构体值,而不需要为其定义一个新的结构体类型。

```go
package main

import (
    "fmt"
)

func main() {
    var user struct{Name string; Age int}
    user.Name = "pprof.cn"
    user.Age = 18
    fmt.Printf("%#v\n", user)
}
```

### 结构体初始化

在Go语言中,结构体可以通过多种方式进行初始化。以下是几种常见的初始化方法:

1. **字面值初始化**

这是最常见的初始化方式,通过在结构体字面值中列出每个字段的值来完成初始化。

```go
type Person struct {
    Name string
    Age  int
}

// 使用字面值初始化
p1 := Person{"Alice", 30}
```

1. **基于键值对的初始化**

如果你希望只初始化部分字段,或者按照不同的顺序初始化字段,可以使用键值对的形式。

```go
p2 := Person{Name: "Bob", Age: 35}
p3 := Person{Age: 40, Name: "Charlie"}
```

1. **值列表初始化**

也可以使用值列表的形式进行初始化,在这种情况下,值的顺序必须与结构体字段的定义顺序一致。

```go
p4 := Person{"David", 45}
```

1. **使用new函数初始化**

使用new函数可以获得一个指向新分配的零值结构体的指针。

```go
p5 := new(Person)
```

1. **使用&和结构体字面值初始化**

采用取地址&操作符和结构体字面值的方式也可以初始化一个结构体指针。

```go
p6 := &Person{"Eve", 50}
```

无论使用哪种初始化方式,如果字段没有提供明确的值,它们都会被自动初始化为对应类型的零值。

### 结构体嵌套

```go
package main

import "fmt"

type contact struct {
	email   string
	phone   string
	address string
}

type person struct {
	name    string
	age     int
	contact contact
}

func main() {
	p := person{
		name: "Alice",
		age:  30,
		contact: contact{
			email:   "alice@example.com",
			phone:   "1234567890",
			address: "123 Main St",
		},
	}

	// 访问最内层的变量
	fmt.Println("Name:", p.name)
	fmt.Println("Age:", p.age)
	fmt.Println("Email:", p.contact.email)
	fmt.Println("Phone:", p.contact.phone)
	fmt.Println("Address:", p.contact.address)
}

```

### 定义的结构体的名称 就是一种新的类型

```go
//User 用户结构体
type User struct {
    Name    string
    Gender  string
    Address //匿名结构体
}
```

在这个定义中，`User` 是一个自定义的结构体类型，可以创建 `User` 类型的变量，比如 `user2`。这样，`User` 就成为了一个新的类型，

### 构造函数 

Go 语言没有像其他面向对象编程语言那样的构造函数的概念。相反,Go 使用以下方式来初始化结构体:

1. **工厂函数**

工厂函数是一种常见的模式,用于创建并初始化结构体。它们通常以 `New` 开头,返回一个结构体的实例。

```go
type Person struct {
    Name string
    Age  int
}

func NewPerson(name string, age int) *Person {
    return &Person{
        Name: name,
        Age:  age,
    }
}

p := NewPerson("Alice", 30)
```

1. **结构体字面值初始化**

如前所述,您可以使用结构体字面值初始化来创建结构体实例。这种方式可以提供更好的灵活性和控制。

```go
p := Person{
    Name: "Bob",
    Age:  35,
}
```

1. **零值初始化**

如果您不提供任何值,结构体字段将自动初始化为零值。零值是每种类型的默认值,如 `0` 用于数值,`""` 用于字符串,`false` 用于布尔值等。

```go
var p Person // p = {Name: "", Age: 0}
```

1. **用 new 函数初始化**

`new` 函数用于分配内存并返回指向新分配内存的指针,并将其初始化为零值。

```go
p := new(Person) // p = &Person{Name: "", Age: 0}
```

尽管 Go 没有真正的构造函数,但使用工厂函数和结构体字面值初始化可以很好地模拟构造函数的行为。工厂函数通常用于封装初始化逻辑,或者在创建结构体实例时执行额外的设置和验证。

### 结构体的继承

```go
package main

import "fmt"

//Animal 动物
type Animal struct {
	name string
}

func (a *Animal) move() {
	fmt.Printf("%s会动！\n", a.name)
}

//Dog 狗
type Dog struct {
	Feet   int8
	Animal //通过嵌套匿名结构体实现继承
}

func (d *Dog) wang() {
	fmt.Printf("%s会汪汪汪~\n", d.name)
}

func main() {
	d1 := Dog{
		Feet: 4,
		Animal: Animal{ //注意嵌套的是结构体指针
			name: "乐乐",
		},
	}
	d1.wang() //乐乐会汪汪汪~
	d1.move() //乐乐会动！
}

```

###传值接收者和指针接收者

在Go语言中，方法是作用在具体类型上的函数，通过为一个类型定义方法，可以实现在该类型上进行操作的行为。方法是将函数与特定类型关联起来的一种方式。

在Go语言中，方法的定义格式如下：

```go
func (receiver Type) methodName(parameters) returnType {
    // 方法实现
}
```

- `receiver`：表示方法所作用的类型，也可以理解为方法的接收者。
- `Type`：表示接收者的类型。
- `methodName`：表示方法的名称。
- `parameters`：表示方法的参数列表。
- `returnType`：表示方法的返回类型。

而在Go语言中，这样的叫法叫类型的接收者

传值接收者和指针接收者

当方法作用于值类型接收者时，Go语言会在代码运行时将接收者的值复制一份。在值类型接收者的方法中可以获取接收者的成员值，但修改操作只是针对副本，无法修改接收者变量本身。使用指针接收者时，方法可以修改接收者结构体的内容。

示例如下：

```go
package main

import "fmt"

type Person struct {
	name string
	age  int8
}

//NewPerson 构造函数
func NewPerson(name string, age int8) *Person {
	return &Person{
		name: name,
		age:  age,
	}
}
func (p Person) Dream() {//传值类型
	fmt.Println("I am ", p.name, " and I am ", p.age, " years old.")
}
func (p *Person) SetAge2(newAge int8) {//指针类型
	p.age = newAge
}

func main() {
	p1 := NewPerson("测试", 25)
	p1.Dream()
	fmt.Println(p1.age) // 25
	p1.SetAge2(30)      // (*p1).SetAge2(30)
	fmt.Println(p1.age) // 25
}

```



### 两种接收状态的区别

在Go语言中，结构体方法的接收者可以是该方法的参数传值（value receiver）或者指针（pointer receiver）。这两种接收方式有一些重要区别：

1. **Value Receiver（传值接收者）**：
   - 当一个方法使用值接收者时，它操作的是该类型的一个副本，而不是原始值本身。
   - 任何对接收者结构体的修改都不会影响原始结构体，因为方法操作的是副本。
   - 适合于不需要在方法中修改接收者的情况，或者接收者是小型值类型的情况。
2. **Pointer Receiver（指针接收者）**：
   - 使用指针接收者时，方法可以修改接收者结构体的内容。
   - 任何对接收者的更改都会影响原始结构体，因为方法操作的是原始结构体的引用。
   - 适合于需要在方法中修改接收者或者接收者是大型结构体的情况，以避免复制整个结构体。

在选择接收者类型时，需要根据具体情况来决定。如果方法需要修改接收者的状态或者接收者是复杂的结构体，通常会选择指针接收者。如果方法仅仅是对接收者进行操作而不需要修改其状态，那么使用值接收者可能更合适。

示例如下：

```go
// 定义一个名为Rectangle的结构体类型
type Rectangle struct {
    length float64
    width  float64
}

// 为Rectangle结构体类型定义一个计算面积的方法（值接收者）
func (r Rectangle) Area() float64 {
    return r.length * r.width
}

// 为Rectangle结构体类型定义一个计算周长的方法（指针接收者）
func (r *Rectangle) Perimeter() float64 {
    return 2 * (r.length + r.width)
}

func main() {
    // 创建一个Rectangle类型的变量
    rect := Rectangle{length: 10.0, width: 5.0}

    // 调用Rectangle的Area()方法
    fmt.Println("Rectangle area:", rect.Area()) // 输出: Rectangle area: 50.0

    // 调用Rectangle的Perimeter()方法
    fmt.Println("Rectangle perimeter:", rect.Perimeter()) // 输出: Rectangle perimeter: 30.0
}
```

### 方法的重写和继承

如果匿名字段实现了一个method，那么包含这个匿名字段的struct也能调用该method。让我们来看下面这个例子

```go

package main

import "fmt"

type Human struct {
	name string
	age int
	phone string
}

type Student struct {
	Human //匿名字段
	school string
}

type Employee struct {
	Human //匿名字段
	company string
}

//在human上面定义了一个method
func (h *Human) SayHi() {
	fmt.Printf("Hi, I am %s you can call me on %s\n", h.name, h.phone)
}

func main() {
	mark := Student{Human{"Mark", 25, "222-222-YYYY"}, "MIT"}
	sam := Employee{Human{"Sam", 45, "111-888-XXXX"}, "Golang Inc"}

	mark.SayHi()
	sam.SayHi()
}
```

重写示例：

```go

package main

import "fmt"

type Human struct {
	name string
	age int
	phone string
}

type Student struct {
	Human //匿名字段
	school string
}

type Employee struct {
	Human //匿名字段
	company string
}

//Human定义method
func (h *Human) SayHi() {
	fmt.Printf("Hi, I am %s you can call me on %s\n", h.name, h.phone)
}

//Employee的method重写Human的method
func (e *Employee) SayHi() {
	fmt.Printf("Hi, I am %s, I work at %s. Call me on %s\n", e.name,
		e.company, e.phone) //Yes you can split into 2 lines here.
}

func main() {
	mark := Student{Human{"Mark", 25, "222-222-YYYY"}, "MIT"}
	sam := Employee{Human{"Sam", 45, "111-888-XXXX"}, "Golang Inc"}

	mark.SayHi()
	sam.SayHi()
}
```



### 为什么做出接受者的区分

在Go语言中，将方法定义为值类型接收者或指针类型接收者之间存在一些区别和考量：

1. 内存开销：使用值类型接收者会在每次方法调用时对接收者进行值拷贝，可能导致内存开销较高，尤其在方法频繁调用时。而使用指针类型接收者可以避免不必要的值拷贝，提高性能。
2. 修改原始值：如果方法需要修改接收者的内部状态或者想要确保对接收者的修改在调用函数外部有效，应该使用指针类型接收者。这是因为值类型接收者只会修改方法接收者的拷贝，而不会修改原始值。
3. 接口实现：如果一个结构体要实现某个接口，如果接口方法使用了指针类型接收者，那么结构体必须使用指针类型接收者来实现这个接口。反之亦然。

根据以上考量，是否选择值类型接收者还是指针类型接收者取决于具体情况和需求。在设计方法时，需要综合考虑这些因素，选择最合适的接收者类型

### 怎么判断是否有返回值

以下是一些常见情况下函数可能会有返回值：

1. 计算型函数：函数执行一些计算操作，并返回计算结果给调用者。

```go
package main

import "fmt"

func add(a, b int) int {
    return a + b
}

func main() {
    result := add(3, 5)
    fmt.Println(result) // 输出8
}
```

1. 查询型函数：函数用于查询数据或状态，并返回查询结果。

```go
package main

import "fmt"

func containsValue(slice []int, value int) bool {
    for _, v := range slice {
        if v == value {
            return true
        }
    }
    return false
}

func main() {
    numbers := []int{1, 2, 3, 4, 5}
    contains := containsValue(numbers, 3)
    fmt.Println(contains) // 输出true
}
```

1. 状态更新函数：函数执行某种操作后，返回更新后的状态或结果。

```go
package main

import "fmt"

func pop(slice []int) (int, []int) {
    if len(slice) == 0 {
        return 0, slice
    }
    last := slice[len(slice)-1]
    slice = slice[:len(slice)-1]
    return last, slice
}

func main() {
    numbers := []int{1, 2, 3, 4, 5}
    popped, numbers := pop(numbers)
    fmt.Println(popped, numbers) // 输出5 [1 2 3 4]
}
```

总的来说，函数有返回值通常是为了将结果传递给调用者或者指示函数执行状态。在Go语言中，函数可以有多个返回值，可以有不同类型的返回值。



在Go语言中，函数可以没有返回值，这种情况下一般用于执行特定逻辑或操作，而不需要返回具体数值或结果。

以下是一些示例情况：

1. 函数用于修改某个变量或状态，但不需要返回任何值。

```go
package main

import "fmt"

func greet(name string) {
    fmt.Println("Hello, " + name + "!")
}

func main() {
    greet("Alice")
}
```

1. 函数执行某种操作，比如打印输出，但不需要返回值。

```go
package main

import "fmt"

func printMessage() {
    fmt.Println("This is a message.")
}

func main() {
    printMessage()
}
```

1. 函数用于修改切片、Map等引用类型数据结构中的值，但不需要返回修改后的结果。

```go
package main

import "fmt"

func addToSlice(slice []int, value int) {
    slice = append(slice, value)
}

func main() {
    numbers := []int{1, 2, 3}
    addToSlice(numbers, 4)
    fmt.Println(numbers) // 输出结果仍然是[1 2 3]
}
```

总而言之，单纯修改和 需要打印出结果，而不需要调用，就可以省略返回值

```go
package main

import "fmt"

func calculateSum(a, b int) {
    sum := a + b
    fmt.Println("Sum of", a, "and", b, "is", sum)
}

func main() {
    calculateSum(3, 5)
}
```

## 其他

### 匿名字段

下面是一个示例来演示匿名字段的作用：

```go
package main

import "fmt"

// 定义一个基础结构体
type Person struct {
	Name string
	Age  int
}

// 定义一个嵌入Person结构体的新结构体
type Student struct {
	Person  // 匿名字段，继承了Person结构体的字段和方法
	School string
	Grade  int
}

func main() {
	// 创建一个Student对象
	s := Student{
		Person: Person{Name: "Alice", Age: 20},
		School: "XYZ School",
		Grade:  10,
	}

	// 访问嵌入的Person结构体字段
	fmt.Println("Name:", s.Name)
	fmt.Println("Age:", s.Age)

	// 调用嵌入的Person结构体方法
	s.Greet()
}

// 定义一个Person结构体的方法
func (p Person) Greet() {
	fmt.Println("Hello, my name is", p.Name)
}
```

在上面的示例中，定义了一个`Person`结构体和一个`Student`结构体，其中`Student`结构体通过匿名字段继承了`Person`结构体的字段和方法。我们可以直接访问`Student`对象中继承的字段，也可以调用继承的方法。运行上面的代码，可以看到输出结果为：

```
Name: Alice
Age: 20
Hello, my name is Alice
```

这个例子展示了通过匿名字段实现代码复用和继承的功能。

### make关键字

make是Go语言中用于创建切片(slice)、映射(map)和通道(channel)的内置函数。它的作用是为给定的类型分配一个初始化的值,避免手动为这些数据结构分配内存空间。

使用make函数创建切片:

```go
slice := make([]int, 5, 10)
```

这个语句创建了一个长度为5,容量为10的int类型切片。第一个参数是切片的类型,第二个参数是长度,第三个参数是可选的容量。如果不指定容量,默认与长度相同。

使用make创建映射:

```go
myMap := make(map[string]int)
```

这个语句创建了一个空的string到int的映射。

使用make创建通道:

```go
ch := make(chan int, 10)
```

这个语句创建了一个容量为10的int类型通道。第二个参数是可选的,用于设置通道的缓冲大小。

make函数比直接使用字面量语法创建这些数据结构更加安全和高效,因为它会确保底层的内存分配和初始化操作正确进行,避免出现nil值。在处理大量数据时,使用make可以显著提高性能。

## 错误和异常处理

### 书写方法

在Go语言中，错误处理是通过返回错误值来处理异常情况的一种机制，而不像其他语言那样使用异常。Go的错误处理机制基于以下两个原则：

1. **错误值**: 函数可以返回一个额外的错误值，通常是`error`类型。如果函数执行成功，错误值为`nil`；如果出现错误，错误值将包含相关的错误信息。这样的处理方式使得错误显式化，可以更好地控制和处理异常情况。
2. **错误检查**: Go鼓励通过检查错误来处理异常情况，而不是简单地忽略错误。通常使用`if err != nil`的方式来检查函数返回的错误，以确保及时发现并处理错误。

下面是一个简单的示例，演示了在Go中如何处理错误：

```go
package main

import (
	"fmt"
	"errors"
)

func divide(a, b float64) (float64, error) {
	if b == 0 {
		return 0, errors.New("division by zero")
	}
	return a / b, nil
}

func main() {
	result, err := divide(10, 0)
	if err != nil {
		fmt.Println("Error:", err)
	} else {
		fmt.Println("Result:", result)
	}
}
```

在上面的示例中，`divide`函数用于执行除法运算，如果除数为0，则返回一个包含错误信息的`error`。在`main`函数中，调用`divide`函数并检查返回的错误，根据错误是否为`nil`来决定如何处理结果。

通过这种方式，Go语言的错误处理提供了一种简洁而有效的方式来处理函数调用可能出现的异常情况，使得代码更加健壮和可靠。

### 自定义err

第一种方法是：**使用errors.New()创建简单错误**

```go
err := errors.New("这是一个简单的错误")
```

第二种方法是：**定义自己的错误类型并实现Error()方法**

```go
package main

import (
    "fmt"
)

type MyError struct {
    Code    int
    Message string
}

func (e *MyError) Error() string {
    return fmt.Sprintf("Error Code: %d, Message: %s", e.Code, e.Message)
}

func Divide(a, b int) (int, error) {
    if b == 0 {
        return 0, &MyError{Code: 100, Message: "Division by zero"}
    }
    return a / b, nil
}

func main() {
    result, err := Divide(10, 0)
    if err != nil {
        fmt.Println("Error :", err)
    } else {
        fmt.Println("Result:", result)
    }
}
```

## 接口

### 接口的实现

简单的说，interface是一组method签名的组合，我们通过interface来定义对象的一组行为。接口就是一组方法的集合，如果一个对象拥有这个集合中全部方法，那么就默认实现了这个接口。

```go
// 定义一个接口Shape，包含计算面积和周长的方法
type Shape interface {
    Area() float64
    Perimeter() float64
}

// 定义一个名为Rectangle的结构体类型
type Rectangle struct {
    length float64
    width  float64
}

// 实现Shape接口的Area方法（值接收者）
func (r Rectangle) Area() float64 {
    return r.length * r.width
}

// 实现Shape接口的Perimeter方法（指针接收者）
func (r *Rectangle) Perimeter() float64 {
    return 2 * (r.length + r.width)
}

func main() {
    // 创建一个Rectangle类型的变量
    rect := Rectangle{length: 10.0, width: 5.0}

    // 将Rectangle类型的变量赋值给Shape接口
    var shape Shape
    shape = rect

    // 调用Shape接口的方法
    fmt.Println("Shape area:", shape.Area())           // 输出: Shape area: 50.0
    fmt.Println("Shape perimeter:", shape.Perimeter()) // 输出: Shape perimeter: 30.0
}
```

```go

package main

import "fmt"

type Human struct {
	name string
	age int
	phone string
}

type Student struct {
	Human //匿名字段
	school string
	loan float32
}

type Employee struct {
	Human //匿名字段
	company string
	money float32
}

//Human实现SayHi方法
func (h Human) SayHi() {
	fmt.Printf("Hi, I am %s you can call me on %s\n", h.name, h.phone)
}

//Human实现Sing方法
func (h Human) Sing(lyrics string) {
	fmt.Println("La la la la...", lyrics)
}

//Employee重载Human的SayHi方法
func (e Employee) SayHi() {
	fmt.Printf("Hi, I am %s, I work at %s. Call me on %s\n", e.name,
		e.company, e.phone)
	}

// Interface Men被Human,Student和Employee实现
// 因为这三个类型都实现了这两个方法
type Men interface {
	SayHi()
	Sing(lyrics string)
}

func main() {
	mike := Student{Human{"Mike", 25, "222-222-XXX"}, "MIT", 0.00}
	paul := Student{Human{"Paul", 26, "111-222-XXX"}, "Harvard", 100}
	sam := Employee{Human{"Sam", 36, "444-222-XXX"}, "Golang Inc.", 1000}
	tom := Employee{Human{"Tom", 37, "222-444-XXX"}, "Things Ltd.", 5000}

	//定义Men类型的变量i
	var i Men

	//i能存储Student
	i = mike
	fmt.Println("This is Mike, a Student:")
	i.SayHi()
	i.Sing("November rain")

	//i也能存储Employee
	i = tom
	fmt.Println("This is tom, an Employee:")
	i.SayHi()
	i.Sing("Born to be wild")

	//定义了slice Men
	fmt.Println("Let's use a slice of Men and see what happens")
	x := make([]Men, 3)
	//这三个都是不同类型的元素，但是他们实现了interface同一个接口
	x[0], x[1], x[2] = paul, sam, mike

	for _, value := range x{
		value.SayHi()
	}
}
```

### 接口与类型的关系

下面是一个类型实现多个接口的示例：

```go
package main

import "fmt"

// 定义接口1
type Driver interface {
	Drive()
}

// 定义接口2
type Navigator interface {
	Navigate()
}

// 定义类型Car
type Car struct{}

// Car类型实现Driver接口
func (c Car) Drive() {
	fmt.Println("Car is driving..")
}

// Car类型实现Navigator接口
func (c Car) Navigate() {
	fmt.Println("Car is navigating..")
}

func main() {
	car := Car{}

	// 类型Car同时实现Driver和Navigator接口
	var driver Driver = car
	driver.Drive()

	var navigator Navigator = car
	navigator.Navigate()
}
```



接下来是多个类型实现同一个接口的示例：

```go
package main

import "fmt"

// 定义接口
type Shape interface {
	Area() float64
}

// 定义类型Rectangle
type Rectangle struct {
	width, height float64
}

// Rectangle类型实现Shape接口
func (r Rectangle) Area() float64 {
	return r.width * r.height
}

// 定义类型Circle
type Circle struct {
	radius float64
}

// Circle类型实现Shape接口
func (c Circle) Area() float64 {
	return 3.14 * c.radius * c.radius
}

func main() {
	rect := Rectangle{width: 10, height: 5}
	circ := Circle{radius: 7}

	shapes := []Shape{rect, circ}

	for _, shape := range shapes {
		fmt.Printf("Area of the shape is: %f\n", shape.Area())
	}
}
```



### 接口的嵌套

接口的嵌套是指一个接口可以包含其他接口，这种方式可以在一个接口定义中合并多个接口的方法集合。当一个接口嵌套了其他接口时，它继承了嵌套的接口的所有方法。这种机制使得接口更加灵活和模块化，允许在一个接口中组合多个功能相关或相似的方法集合。

```go
package main

import "fmt"

// 定义父接口
type Animal interface {
	Eat()
}

// 定义子接口，嵌套父接口
type Pet interface {
	Animal
	Play()
}

// 定义Dog类型
type Dog struct {
	Name string
}

// Dog类型实现Eat方法
func (d Dog) Eat() {
	fmt.Println(d.Name, "is eating..")
}

// Dog类型实现Play方法
func (d Dog) Play() {
	fmt.Println(d.Name, "is playing..")
}

func main() {
	dog := Dog{Name: "Buddy"}

	// dog类型同时实现了Animal和Pet接口
	var animal Animal = dog
	animal.Eat()

	var pet Pet = dog
	pet.Eat()
	pet.Play()
}
```

### 空接口

空接口是指没有定义任何方法的接口。因此任何类型都实现了空接口。

空接口类型的变量可以存储任意类型的变量。

```go
package main

import (
	"fmt"
)

func main() {

	// 定义a为空接口
	var a interface{}
	var i int = 5
	// s := "Hello world"
	// a可以存储任意类型的数值
	a = i
	// a = s
	fmt.Println(a)

}
```

## 反射

在 Go 语言中，**反射**（Reflection）是指程序在运行时检查自身的结构，特别是类型的能力，并有能力在运行时操作对象的属性和方法。Go 的反射机制让程序能够检查类型、调用方法、获取结构体的字段和标签等信息，而这些通常在编译时是无法确定的。

通过反射，你可以做到以下几件事情：

1. 获取任意值的类型（Type）信息。
2. 创建任意类型的实例。
3. 调用任意类型的方法。
4. 获取和修改任意结构体的字段值。

示例：

```go
package main

import (
	"fmt"
	"reflect"
)

func main() {
	x := 3.4
	fmt.Println("Type of x:", reflect.TypeOf(x))
	fmt.Println("Value of x:", reflect.ValueOf(x))
}

```

结果为

```go
Type of x: float64
Value of x: 3.4
```

## 并发

### goroutine

goroutine是Go并行设计的核心。goroutine说到底其实就是协程，但是它比线程更小，十几个goroutine可能体现在底层就是五六个线程，Go语言内部帮你实现了这些goroutine之间的内存共享。执行goroutine只需极少的栈内存(大概是4~5KB)，当然会根据相应的数据伸缩。也正因为如此，可同时运行成千上万个并发任务。goroutine比thread更易用、更高效、更轻便。

goroutine是通过Go的runtime管理的一个线程管理器。goroutine通过`go`关键字实现了，其实就是一个普通的函数

```go
package main

import (
	"fmt"
	"time"
)

func printNumbers() {
	for i := 1; i <= 5; i++ {
		fmt.Printf("%d ", i)
		time.Sleep(100 * time.Millisecond) // 模拟一些工作
	}
}

func main() {
	// 启动一个goroutine来执行printNumbers函数
	go printNumbers()

	// 主goroutine继续执行这里的代码
	for j := 6; j <= 10; j++ {
		fmt.Printf("%d ", j)
		time.Sleep(100 * time.Millisecond) // 模拟一些工作
	}

	// 等待一段时间，以便观察goroutine的输出
	time.Sleep(1 * time.Second)
}

```

### Channel

总的来说，channel是Go语言中用于处理并发通信的重要工具，它提供了一种简单而强大的机制来协调不同goroutines之间的交互，避免了共享内存并发访问的问题。

下面是一个生产者消费者模型的示例

```go
package main

import (
	"fmt"
	"time"
)

func worker(workerID int, jobs <-chan int, results chan<- int) {
	for j := range jobs {
		fmt.Println("worker", workerID, "processing job", j)
		time.Sleep(time.Second)
		results <- j
	}
}

func main() {
	jobs := make(chan int, 100)
	results := make(chan int, 100)

	// 启动3个工作者goroutine
	for w := 1; w <= 3; w++ {
		go worker(w, jobs, results)
	}

	// 发送9个任务
	for j := 1; j <= 9; j++ {
		jobs <- j
	}
	close(jobs)

	// 收集处理的结果
	for j := range results {
		fmt.Println("已经接收到任务", j)
	}
}

```

但是代码存在一个风险，主要问题在于`results`通道的写入操作和`results`通道的读取操作之间存在潜在的死锁风险。在`worker`函数中，结果被发送到`results`通道后，`main`函数中使用了`range results`的方式来接收结果。

这种情况下，如果`results`通道中的数据没有被完全接收，主程序就会一直等待新的数据，从而导致死锁。这是因为`range results`会一直等待通道关闭，而在这种情况下，通道并不会关闭。

为避免死锁，可以修改`main`函数，使用一个计数器来跟踪已经接收的结果数量，以确保在接收完所有结果后正常退出。

下面是修改后的代码：

```go
package main

import (
	"fmt"
	"time"
	
)

func worker(workerID int, jobs <-chan int, results chan<- int) {
	for j := range jobs {
		fmt.Println("worker", workerID, "processing job", j)
		time.Sleep(time.Second)
		results <- j
	}
}

func main() {
	jobs := make(chan int, 100)
	results := make(chan int, 100)

	// 启动3个工作者goroutine
	for w := 1; w <= 3; w++ {
		go worker(w, jobs, results)
	}

	// 发送9个任务
	for j := 1; j <= 9; j++ {
		jobs <- j
	}
	close(jobs)

	// 收集处理的结果
	totalJobs := 9
	received := 0
	for received < totalJobs {
		select {
		case j := <-results:
			received++
			fmt.Println("已经接收到任务", j)
		}
	}
}

```

### select关键字

`select` 是 Go 语言中的一个控制结构，用于处理一个或多个 channel 操作。它类似于 `switch` 语句，但用于通信操作。`select` 语句使一个 Go 程序可以等待多个通信操作。当多个 `case` 中的通信操作都可以进行时，`select` 会随机选择一个执行。

在 `select` 语句中，每个 `case` 都是一个通信操作，可以是 `send`、`receive` 或 `default`。`select` 会阻塞，直到至少有一个 `case` 可以运行，然后执行这个 `case`。如果有多个 `case` 可以运行，它们会以伪随机的方式选择一个执行。

使用 `select` 可以避免因为单个通道阻塞而导致整个程序阻塞的情况，使得程序更加灵活。这在需要同时处理多个 channel 通信时非常有用，例如通过 `select` 可以实现超时控制、非阻塞的 channel 操作等。

下面是一个简单的示例，演示了如何在 `select` 语句中处理多个 channel 操作：

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	ch1 := make(chan string)
	ch2 := make(chan string)

	go func() {
		time.Sleep(1 * time.Second)
		ch1 <- "one"
	}()

	go func() {
		time.Sleep(2 * time.Second)
		ch2 <- "two"
	}()

	for i := 0; i < 2; i++ {
		select {
		case msg1 := <-ch1:
			fmt.Println("received", msg1)
		case msg2 := <-ch2:
			fmt.Println("received", msg2)
		}
	}
}
```

在这个示例中，`select` 会等待 `ch1` 和 `ch2` 中的数据到达，然后分别处理两个 channel 的数据。
