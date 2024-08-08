#







## 函数定义

在JavaScript中，函数可以通过多种方式进行定义。以下是几种常见的函数定义方法：

1. 函数声明（Function Declaration）：

   ```javascript
   function functionName(parameters) {
     // 函数体
   }
   ```

   使用函数声明方式定义的函数可以在其所在作用域内随处调用。

2. 函数表达式（Function Expression）：

   ```javascript
   var functionName = function(parameters) {
     // 函数体
   }
   ```

   使用函数表达式方式定义的函数可以通过变量名调用，且只能在定义之后的代码行中使用。

3. 箭头函数（Arrow Function）：

   ```javascript
   var functionName = (parameters) => {
     // 函数体
   }
   ```

   箭头函数是ES6引入的一种更简洁的函数定义方式，使用箭头函数可以更方便地定义匿名函数或简单的函数。

4. 函数构造器（Function Constructor）：

   ```javascript
   var functionName = new Function('parameters', 'functionBody');
   ```

   使用函数构造器可以动态地创建函数，但它通常不被推荐使用，因为会引入安全性和性能方面的问题。

## JavaScript 对象



这段代码把一个*单一值*（porsche）赋给名为 car 的*变量*：

```javascript
var car = "porsche";
```

对象也是变量。但是对象包含很多值。

这段代码把*多个值*（porsche, 911, white）赋给名为 car 的*变量*：

```javascript
var car = {type:"porsche", model:"911", color:"white"};
```



值以*名称:值*对的方式来书写（名称和值由冒号分隔）。

JavaScript 对象是*被命名值*的容器。

## 对象属性

（JavaScript 对象中的）名称:值对被称为*属性*。

```javascript
var person = {firstName:"Bill", lastName:"Gates", age:62, eyeColor:"blue"};
```

| 属性      | 属性值 |
| --------- | ------ |
| firstName | Bill   |
| lastName  | Gates  |
| age       | 62     |
| eyeColor  | blue   |

## 对象方法

对象也可以有*方法*。方法是在对象上执行的*动作*。方法以*函数定义*被存储在属性中。

| 属性      | 属性值                                                    |
| --------- | --------------------------------------------------------- |
| firstName | Bill                                                      |
| lastName  | Gates                                                     |
| age       | 62                                                        |
| eyeColor  | blue                                                      |
| fullName  | function() {return this.firstName + " " + this.lastName;} |

示例

```
var person = { firstName: "Bill", lastName : "Gates", id : 678, fullName : function() { return this.firstName + " " + this.lastName; }};
```

## this 关键词

在函数定义中，`this` 引用该函数的“拥有者”。

在上面的例子中，`this` 指的是“拥有” fullName 函数的 *person 对象*。

换言之，`this.firstName` 的意思是 *this 对象*的 firstName 属性。

请在 JS this 关键词这一章学习更多有关 this 关键词的知识。

示例

```javascript
const obj = {
    foo: {
        great: function () {
            console.log("Great!");
        }
    },
    greet: function () {
        console.log(`Hello, ${this.foo.great()}!`);
    }
};

obj.greet();
```

```javascript
function Person(name) {
  this.name = name;
}

const john = new Person("John");
console.log(john.name); // 输出 "John"
```

箭头函数中的 `this` 值是在定义函数时确定的，它会捕获所在上下文的 `this` 值，并在函数内部保持不变。

```javascript
const obj = {
  name: "John",
  greet: function() {
    setTimeout(() => {
      console.log(`Hello, ${this.name}!`);
    }, 1000);
  }
};

obj.greet(); // 输出 "Hello, John!"（箭头函数捕获了 greet 方法中的 this 值）
```

```javascript
var foo = function () {
    this.name = 'foo';
    this.getName = function () {
        return this.name;
    }
}

var obj = new foo();
var result = obj.getName();
console.log(result);
```

## 显式绑定和隐式绑定

在JavaScript中，显式绑定和隐式绑定是两种常见的函数调用方式。

1. 显式绑定（Explicit Binding）：
   通过使用`call()`、`apply()`或`bind()`方法，可以显式地指定函数内部的`this`关键字的值。

   - `call()`方法接受一个对象作为第一个参数，后续参数为函数的参数列表，立即调用函数并将指定的对象绑定到`this`上。
   - `apply()`方法与`call()`类似，但接受一个对象作为第一个参数，第二个参数为一个数组或类数组对象，数组中的每个元素将作为参数传递给函数。
   - `bind()`方法创建一个新的函数，将指定的对象绑定到新函数的`this`上，而不立即调用该函数。

   以下是显式绑定的示例：

   ```javascript
   function greet() {
     console.log(`Hello, ${this.name}!`);
   }
   
   var person = {
     name: 'John'
   };
   
   greet.call(person); // 输出：Hello, John!
   greet.apply(person); // 输出：Hello, John!
   
   var boundGreet = greet.bind(person);
   boundGreet(); // 输出：Hello, John!
   ```

2. 隐式绑定（Implicit Binding）：
   在隐式绑定中，函数的`this`关键字会隐式地绑定到一个上下文对象上，通常是调用该函数的对象。

   ```javascript
   var person = {
     name: 'John',
     greet: function() {
       console.log(`Hello, ${this.name}!`);
     }
   };
   
   person.greet(); // 输出：Hello, John!
   ```

   在上面的例子中，`person`对象的`greet`方法中的`this`关键字指向了`person`对象本身。

   需要注意的是，当使用箭头函数时，它会继承外层作用域的`this`值，而不会创建自己的`this`。因此，箭头函数没有自己的隐式绑定。

## 默认绑定（Default Binding）：

当函数以独立函数调用的形式执行时，且没有使用显式绑定，隐式绑定或箭头函数，`this`关键字会被默认绑定到全局对象（在浏览器环境中为`window`对象）或严格模式下的`undefined`。

```javascript
function greet() {
  console.log(`Hello, ${this.name}!`);
}

var name = 'John';
greet(); // 输出：Hello, John!
```

## 箭头函数

箭头函数（Arrow Function）是ES6引入的一种新的函数声明方式，它提供了一种更简洁的语法来定义函数。

```javascript
// 无参数的箭头函数
const greet = () => {
  console.log("Hello!");
};

// 带参数的箭头函数
const multiply = (a, b) => {
  return a * b;
};

// 单条语句的箭头函数
const square = x => x * x;

// 箭头函数作为回调函数
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map(num => num * 2);

// 使用箭头函数简化回调函数
setTimeout(() => {
  console.log("Delayed message");
}, 1000);
```



## 对象定义

我们定义（创建）了一个 JavaScript 对象：

示例

```javascript
var person = { firstName:"Bill", lastName:"Gates", age:50, eyeColor:"blue"};
```

## 访问对象属性

您能够以两种方式访问属性：

```javascript
objectName.propertyName
```

或者

```javascript
objectName["propertyName"]
```

示例：

```javascript
person.lastName
person["lastName"];
```



## 访问对象方法

您能够通过如下语法访问对象方法：

```javascript
objectName.methodName()
```

实例

```javascript
name = person.fullName();
```

如果您*不使用 ()* 访问 fullName 方法，则将返回*函数定义*：

实例

```javascript
name = person.fullName;
```

方法实际上是以属性值的形式存储的函数定义。

## 数组也是一个对象，属性名可计算

```javascript
var prefix = "foo";

var myObject = {
    [prefix + "bar"]: "hello",
    [prefix + "baz"]: "world"
};
console.log(myObject["foobar"])
// myObject["foobar"]; // hello
console.log(myObject["foobaz"])
// myObject["foobaz"]; // world
```



## 构造函数

JavaScript构造函数是一种特殊类型的函数，用于创建和初始化对象。它们通常与`new`关键字一起使用，以便在内存中实例化一个新对象并将其初始化为特定类型的实例。

构造函数的命名通常以大写字母开头，以便与普通函数进行区分。当使用`new`关键字调用构造函数时，JavaScript会执行以下操作：

1. 创建一个空对象。
2. 将新对象的原型链接到构造函数的原型对象（通过`prototype`属性）。
3. 将构造函数的上下文（`this`关键字）设置为新对象。
4. 执行构造函数内部的代码，以便初始化对象的属性和方法。
5. 如果构造函数没有显式返回一个对象，则将新对象作为结果返回。

通过使用构造函数，可以创建多个相似的对象，这些对象共享相同的属性和方法。构造函数可以接受参数，以便在创建对象时传递初始值。

示例：

```javascript
function Person(name, age) {
  this.name = name;
  this.age = age;
  
  this.greet = function() {
    console.log(`Hello, my name is ${this.name} and I am ${this.age} years old.`);
  };
}

// 使用构造函数创建对象
const person1 = new Person("John", 25);
const person2 = new Person("Jane", 30);

// 调用对象的方法
person1.greet(); // 输出: Hello, my name is John and I am 25 years old.
person2.greet(); // 输出: Hello, my name is Jane and I am 30 years old.
```

在上面的示例中，`Person` 是一个构造函数，它接受 `name` 和 `age` 参数，并将它们分配给新创建的对象的属性。每个对象都有一个 `greet` 方法，用于输出对象的信息。通过使用 `new` 关键字和构造函数，我们可以创建多个 `Person` 对象并调用它们的方法。

## with关键字

`with`关键字在JavaScript中用于创建一个临时的作用域，并在该作用域内执行指定的代码块。它通常用于处理资源的获取和释放，例如文件的打开和关闭。

```javascript
with (expression) {
  // 在作用域中执行的代码块
}
```

在`with`语句中，`expression`是一个返回对象的表达式。该对象可以是任何具有属性和方法的对象。

`with`语句的作用是将指定的对象添加到作用域链的前端，使得在代码块内部可以直接访问该对象的属性和方法，而无需通过对象的名称进行限定。当代码块执行完毕后，作用域链会恢复到之前的状态。

下面是一个使用`with`关键字的示例，假设有一个名为`book`的对象，该对象具有`title`和`author`属性：

```javascript
var book = {
  title: "JavaScript Book",
  author: "John Doe"
};

// 使用 with 语句访问对象的属性
with (book) {
  console.log("Title: " + title);   // 直接访问 title 属性
  console.log("Author: " + author); // 直接访问 author 属性
}

// 在 with 语句外部访问对象的属性
console.log("Title: " + book.title);     // 使用对象名称访问 title 属性
console.log("Author: " + book.author);   // 使用对象名称访问 author 属性
```

在上面的示例中，`with`语句将`book`对象添加到作用域链中，使得在代码块内部可以直接访问`title`和`author`属性。在`with`语句外部，需要使用对象名称来访问这些属性。

## 异常处理

`try`/`catch`语句用于捕获和处理JavaScript代码中的异常（即错误）。它允许你编写能够处理异常情况的代码，并提供了一种控制流的机制，使得即使发生异常，程序也可以继续执行而不会中断。

`try`/`catch`语句的基本语法如下：

```javascript
try {
  // 可能会抛出异常的代码块
} catch (error) {
  // 异常处理代码
}
```

在`try`块中，你可以放置可能会抛出异常的代码。如果在执行`try`块中的代码时发生了异常，那么程序的控制流将立即转移到`catch`块，并且异常对象将作为参数传递给`catch`块中的`error`参数。

在`catch`块中，你可以编写用于处理异常的代码逻辑。这可以包括打印错误消息、记录日志、恢复操作状态或执行其他适当的操作。通过捕获异常并在`catch`块中处理它，你可以防止异常导致程序的崩溃或不可预测的行为。

以下是一个使用`try`/`catch`语句的示例，演示了如何处理可能引发异常的代码：

```javascript
try {
  // 可能会抛出异常的代码
  var result = someFunction(); // 假设 someFunction() 可能抛出异常
  console.log(result); // 如果没有异常，打印结果
} catch (error) {
  // 异常处理代码
  console.log("An error occurred: " + error.message);
}
```

在上面的示例中，`try`块中的`someFunction()`调用可能会抛出异常。如果没有异常发生，那么`result`的值将被打印出来。如果发生异常，程序的控制流将转移到`catch`块，在控制台输出错误消息。

通过使用`try`/`catch`语句，你可以更好地控制和处理JavaScript代码中的异常情况，提高程序的可靠性和健壮性。

# JavaScript 事件



**HTML 事件是发生在 HTML 元素上的“事情”。**

**当在 HTML 页面中使用 JavaScript 时，JavaScript 能够“应对”这些事件。**

## HTML 事件

HTML 事件可以是浏览器或用户做的某些事情。

下面是 HTML 事件的一些例子：

- HTML 网页完成加载
- HTML 输入字段被修改
- HTML 按钮被点击

通常，当事件发生时，用户会希望做某件事。

JavaScript 允许您在事件被侦测到时执行代码。

*通过 JavaScript 代码*，HTML 允许您向 HTML 元素添加事件处理程序。

使用单引号：

```javascript
<element event='一些 JavaScript'>
```

使用双引号：

```javascript
<element event="一些 JavaScript">
```

在下面的例子中，`onclick` 属性（以及代码）被添加到 `<button>` 元素：

示例

```javascript
<button onclick='document.getElementById("demo").innerHTML=Date()'>现在的时间是？</button>
```

[亲自试一试](https://www.w3school.com.cn/tiy/t.asp?f=eg_js_event_onclick)

在上面的例子中，JavaScript 代码改变了 id="demo" 的元素的内容。

在接下来的例子中，代码（使用 `this.innerHTML`）改变了其自身元素的内容：

示例

```javascript
<button onclick="this.innerHTML=Date()">现在的时间是？</button>
```

[亲自试一试](https://www.w3school.com.cn/tiy/t.asp?f=eg_js_event_onclick_this)

JavaScript 代码通常有很多行。事件属性调用函数更为常见：

示例

```javascript
<button onclick="displayDate()">现在的时间是？</button>
```



## 常见的 HTML 事件

下面是一些常见的 HTML 事件：

| 事件        | 描述                         |
| ----------- | ---------------------------- |
| onchange    | HTML 元素已被改变            |
| onclick     | 用户点击了 HTML 元素         |
| onmouseover | 用户把鼠标移动到 HTML 元素上 |
| onmouseout  | 用户把鼠标移开 HTML 元素     |
| onkeydown   | 用户按下键盘按键             |
| onload      | 浏览器已经完成页面加载       |

示例

```javascript
<button onmouseover="this.innerHTML=Date()" onmouseout="this.innerHTML='鼠标移出'">时间是？</button>
```



```javascript
<h1>按键事件示例</h1>

<input type="text" id="myInput" onkeydown="myFunction(event)">

<p id="demo"></p>

<script>
function myFunction(event) {
  document.getElementById("demo").innerHTML = "按键码: " + event.keyCode;
}
</script>
```



```javascript
<h1>输入框内容改变事件示例</h1>

<input type="text" id="myInput" onchange="myFunction()">

<p id="demo">输入框内容将在这里显示</p>

<script>
function myFunction() {
  var inputText = document.getElementById("myInput").value;
  document.getElementById("demo").innerHTML = "输入框内容已改变为: " + inputText;
}
</script>
```

**当用户在输入框中输入内容并移开焦点，或者按下回车键时，会触发 `myFunction` 函数，该函数将显示输入框内容已经改变为的内容。**



# 闭包

闭包在 JavaScript 中具有多种作用和优点，包括但不限于：

1. **数据封装和隐藏：** 闭包可以帮助实现数据私有化，将变量和函数封装在内部函数中，只暴露必要的接口给外部。这样可以防止外部直接访问内部变量，提高代码安全性。
2. **保存状态：** 闭包可以在外部函数执行完毕后仍然保持对外部函数作用域中变量的引用，从而能够保持状态。这在一些需要保持状态的场景下非常有用，比如计数器、事件处理函数等。
3. **实现模块化：** 闭包可以用于实现模块化，将相关的变量和函数封装在一个闭包中，避免全局污染，提高代码的可维护性和复用性。
4. **延迟执行：** 闭包可以延迟函数的执行，将函数作为返回值返回，等到调用闭包时才执行函数，实现延迟执行的效果。
5. **实现高阶函数：** 闭包可以作为高阶函数的参数或返回值，实现函数式编程中的高阶函数功能，如函数柯里化、函数组合等。

总的来说，闭包是 JavaScript 中非常强大和灵活的特性，可以帮助解决许多问题，并且为编写复杂的程序提供了便利。但需要注意，滥用闭包可能导致内存泄漏等问题，因此在使用闭包时需要谨慎处理。

## 数据封装和隐藏示例：

```javascript
function createCounter() {
    let count = 0;

    function increment() {
        count++;
    }

    function getCount() {
        return count;
    }

    return {
        increment,
        getCount
    };
}

const counter = createCounter();
counter.increment();
console.log(counter.getCount()); // 输出 1
```

在这个示例中，`createCounter` 函数返回一个包含两个函数 `increment` 和 `getCount` 的对象，这两个函数可以操作和获取 `count` 变量的值。外部无法直接访问 `count` 变量，从而实现了数据封装和隐藏。

保存状态示例：

```javascript
function createAdder(initial) {
    return function(num) {
        initial += num;
        return initial;
    };
}

const adder = createAdder(5);
console.log(adder(3)); // 输出 8
console.log(adder(2)); // 输出 10
```

在这个示例中，`createAdder` 函数返回一个闭包，闭包中的 `initial` 变量可以在多次调用中保持状态。每次调用 `adder` 函数时，`initial` 的值都会保留并更新，从而实现了保存状态的功能。

实现模块化示例：

```javascript
const module = (function() {
    let privateVar = "私有变量";

    function privateFunction() {
        console.log("访问私有变量：" + privateVar);
    }

    return {
        publicFunction: function() {
            privateFunction();
        }
    };
})();

module.publicFunction(); // 输出 "访问私有变量：私有变量"
// 尝试访问私有变量和函数会报错
// console.log(module.privateVar);
// module.privateFunction();
```

在这个示例中，使用立即执行函数创建了一个模块，模块中包含了一个私有变量 `privateVar` 和一个私有函数 `privateFunction`，同时返回一个包含公共函数 `publicFunction` 的对象。这样就实现了模块化，外部只能访问到公共函数，无法直接访问私有变量和函数。

这些示例展示了闭包在不同场景下的应用，可以帮助实现数据封装和隐藏、保存状态以及模块化等功能。





## JavaScript - HTML DOM 方法

**HTML DOM 方法是您能够（在 HTML 元素上）执行的\*动作\*。**

**HTML DOM 属性是您能够设置或改变的 HTML 元素的\*值\*。**

## DOM 编程界面

HTML DOM 能够通过 JavaScript 进行访问（也可以通过其他编程语言）。

在 DOM 中，所有 HTML 元素都被定义为*对象*。

编程界面是每个对象的属性和方法。

*属性*是您能够获取或设置的值（就比如改变 HTML 元素的内容）。

*方法*是您能够完成的动作（比如添加或删除 HTML 元素）。

实例

下面的例子改变了 id="demo" 的 <p> 元素的内容：

```javascript
<html><body><p id="demo"></p><script>document.getElementById("demo").innerHTML = "Hello World!";</script></body></html>
```

[亲自试一试](https://www.w3school.com.cn/tiy/t.asp?f=eg_js_dom_method)

在上面的例子中，`getElementById` 是*方法*，而 `innerHTML` 是*属性*。

## getElementById 方法

访问 HTML 元素最常用的方法是使用元素的 id。

在上面的例子中，`getElementById` 方法使用 id="demo" 来查找元素。

## innerHTML 属性

获取元素内容最简单的方法是使用 `innerHTML` 属性。

`innerHTML` 属性可用于获取或替换 HTML 元素的内容。

`innerHTML` 属性可用于获取或改变任何 HTML 元素，包括 `<html>` 和 `<body>`。

# JavaScript 类

类的基本语法

```javascript
class ClassName {
    constructor([param1, param2, ...]) {
        // 初始化属性
        this.property1 = param1;
        this.property2 = param2;
        // ...
    }

    // 实例方法
    methodName() {
        // 方法体
    }

    // 静态方法
    static staticMethodName() {
        // 方法体
    }
}
```

## 构造函数

```javascript
class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
}
```

## 类的封装

在JavaScript中，封装通常通过创建对象字面量、构造函数或类来实现。以下是通过对象字面量和构造函数两种方式来实现封装的示例：

### 通过对象字面量实现封装

对象字面量可以用来创建封装了数据和方法的对象。这种方法没有类的概念，但是可以使用 `this` 关键字来引用对象的属性。

```javascript
function Employee(name, position) {
    this.name = name;
    this.position = position;
}

Employee.prototype.getDetails = function() {
    return `Name: ${this.name}, Position: ${this.position}`;
};

const employee1 = new Employee('Alice', 'Software Engineer');
console.log(employee1.getDetails()); // 输出: Name: Alice, Position: Software Engineer
```

### 通过构造函数实现封装

构造函数提供了一种更结构化的方式来实现封装。构造函数通常用于初始化对象的属性，并通过原型链（`prototype`）定义方法。

```javascript
function Employee(name, position) {
    this.name = name;
    this.position = position;
}

Employee.prototype.getDetails = function() {
    return `Name: ${this.name}, Position: ${this.position}`;
};

var employee1 = new Employee('Bob', 'Manager');
console.log(employee1.getDetails()); // 输出: Name: Bob, Position: Manager
```

### 通过ES6类实现封装

JavaScript ES6引入了类来封装数据和方法，提供了更现代的语法。

```javascript
class Employee {
    constructor(name, position) {
        this.name = name;
        this.position = position;
    }

    getDetails() {
        return `Name: ${this.name}, Position: ${this.position}`;
    }
}

const employee2 = new Employee('Charlie', 'Intern');
console.log(employee2.getDetails()); // 输出: Name: Charlie, Position: Intern
```

在这些示例中，我们展示了如何通过不同的方式来封装数据和方法。通过使用类或构造函数，我们可以使代码更具可读性，更容易维护，并且遵循面向对象编程的原则。

## 类的继承

在JavaScript中，类的继承是通过实现ES6引入的构造函数的重写和原型链来实现的。类的继承允许子类继承父类的属性和方法，并可以通过重写或扩展这些属性和方法来添加特定的行为，从而实现代码的复用和模块化。

#### 实现机制

在基于原型链的继承中，一切对象都通过原型链来相互联接。每个对象都有一个[[Prototype]]属性，它指向其原型。构造函数的原型对象（`Function.prototype`）通常用于存储类的公共属性和方法，而实例的原型（`instance`的原型）则包含实例特有的属性和方法。

```javascript
class Parent {
    constructor(name) {
        this.name = name;
    }

    greet() {
        console.log(`Hello, I am ${this.name}`);
    }
}

class Child extends Parent {
//     // 子类可以重写父类的方法
//     greet() {
//         console.log(`Hi, my name is ${this.name}. Nice to meet you!`);
//     }
}

const childInstance = new Child("Alice");
childInstance.greet(); // 输出: "Hi, my name is Alice. Nice to meet you!"
```

同样的，注释加上实现了对父类方法的重写

在上述例子中，`Child` 类通过扩展 `Parent` 类来继承其属性和方法。当 `Child` 实例调用 `greet()` 方法时，JavaScript 引擎会查找 `Child.prototype.greet`，如果找不到，则会向上查找父类的原型，直到找到或到达最顶级的 `Object.prototype`。

## 私有属性

在JavaScript中，私有属性是那些不能直接从类的外部访问的属性。虽然JavaScript没有原生的私有属性支持（即在类内部声明的属性在外部无法直接访问），但可以使用一些技巧来模拟私有属性的行为。

### 模拟私有属性的方法

一种常见的方法是在类内部使用双下划线（`__`）前缀来声明私有属性。

```javascript
class PrivateClass {
    
    name = "John Doe";
    constructor() {
        // 私有属性
        this.__privateProperty = "This is a private property";
    }

    getPrivateProperty() {
        return this.__privateProperty;
    }
    setPrivateProperty(value) {
        this.__privateProperty = value;
    }
}
const myPrivateClass = new PrivateClass();
console.log(myPrivateClass.name);  // 输出: "John Doe"
console.log(myPrivateClass.getPrivateProperty());  // 输出: "This is a private property"

myPrivateClass.setPrivateProperty("New Private Value");
console.log(myPrivateClass.getPrivateProperty());  // 输出: "New Private Value"
```

## 静态方法

静态方法是类中的一种特殊方法，它不依赖于类的实例对象而存在。它们直接属于类本身，不需要通过创建类的实例即可被调用。

在类中，还可以定义静态方法（`static` 关键字），这些方法属于类本身，而不是类的实例。

```javascript
class MathUtils {
    static add(a, b) {
        return a + b;
    }
}

console.log(MathUtils.add(3, 4)); // 输出: 7
```

## prototype

在JavaScript中，`prototype`是一个属性，它允许我们为构造函数（即创建新对象的函数）定义共享的方法和属性。当我们使用 `new` 关键字来创建基于某个构造函数的对象时，这个构造函数的 `prototype` 属性会自动变成新创建的对象的原型。

为何使用 `prototype`？

1. **方法共享**：通过 `prototype`，我们可以将方法定义在构造函数上，这样所有基于该构造函数创建的对象都可以共享这些方法。这种方式简化了代码的复用和维护。
2. **原型链**：`prototype` 是实现 JavaScript 的原型链（prototype chain）的基础。通过原型链，我们可以实现对象的继承，使一个对象可以通过访问另一个对象的方法和属性来扩展自己的功能。

示例

```javascript
function Person(name) {
    this.name = name;
}

Person.prototype.greet = function() {
    console.log(`Hello, my name is ${this.name}.`);
};

const person1 = new Person('Alice');
const person2 = new Person('Bob');

person1.greet(); // 输出: Hello, my name is Alice.
person2.greet(); // 输出: Hello, my name is Bob.
```

在这个例子中，`Person.prototype.greet` 是 `Person` 类的一个方法，所有 `Person` 的实例（`person1` 和 `person2`）都能访问并调用这个方法。这是通过 `prototype` 属性实现的，每个基于 `Person` 创建的新对象都会继承这个 `prototype` 对象的属性和方法。

原型链：

- 每个对象都有一个 `[[Prototype]]` 属性，指向其原型（默认是 `Object.prototype`）。
- 如果原型是一个构造函数的对象，它的原型（`[[Prototype]]`）将指向该构造函数的 `prototype`。
- 这种链式结构允许JavaScript通过原型链来查找属性和方法，实现了动态的属性查找机制。

## 多态

在JavaScript中，多态是一种允许我们使用一个类的实例来表示多个类型的能力。虽然JavaScript本身不支持像Java或C++那样的严格多态，但通过原型链和函数的柯里化特性，我们仍然可以模拟多态的行为。下面是一个使用JavaScript实现多态的例子：

### 创建一个动物类（Animal）

首先，我们创建一个基本的动物类 `Animal`，它可以包含一些通用的方法，如 `sound()` 和 `eat()`，这样所有子类都可以调用这些方法。

```javascript
class Animal {
    constructor(name) {
        this.name = name;
    }

    sound() {
        console.log("Making a generic sound...");
    }

    eat() {
        console.log(`${this.name} is eating.`);
    }
}
```

### 创建子类：猫（Cat）

然后，我们可以创建一个继承自 `Animal` 类的子类 `Cat`。

```javascript
class Cat extends Animal {
    sound() {
        console.log("Meow!");
    }

    play() {
        console.log(`${this.name} is playing with a toy.`);
    }
}
```

### 创建子类：狗（Dog）

同样，我们可以创建另一个子类 `Dog`。

```javascript
class Dog extends Animal {
    sound() {
        console.log("Woof!");
    }

    fetch() {
        console.log(`${this.name} is fetching a ball.`);
    }
}
```

### 使用多态示例

现在，我们可以使用多态来调用不同动物的行为：

```javascript
const cat = new Cat('Fluffy');
const dog = new Dog('Buddy');

// 调用 eat() 方法，所有动物都可以做这个动作
cat.eat();
dog.eat();

// 调用 sound() 方法，不同动物发出不同声音
cat.sound();
dog.sound();

// 调用子类特有的方法
cat.play(); // 只有猫可以做这个动作
dog.fetch(); // 只有狗可以做这个动作
```

### 多态与继承的关系：

多态和继承是面向对象编程中互补的概念，多态关注运行时行为的多样性，而继承则关注类结构和代码重用。说人话就是：

*一个强调都有爹妈基因，一个强调可以基因突变，每个人都是不一样的烟火。*

# JavaScript HTML DOM 文档

**HTML DOM 文档对象是您的网页中所有其他对象的拥有者。**

## HTML DOM Document 对象

文档对象代表您的网页。

如果您希望访问 HTML 页面中的任何元素，那么您总是从访问 document 对象开始。

下面是一些如何使用 document 对象来访问和操作 HTML 的实例。

## 查找 HTML 元素

| 方法                                    | 描述                   |
| --------------------------------------- | ---------------------- |
| document.getElementById(*id*)           | 通过元素 id 来查找元素 |
| document.getElementsByTagName(*name*)   | 通过标签名来查找元素   |
| document.getElementsByClassName(*name*) | 通过类名来查找元素     |

## 改变 HTML 元素

| 方法                                       | 描述                   |
| ------------------------------------------ | ---------------------- |
| element.innerHTML = *new html content*     | 改变元素的 inner HTML  |
| element.attribute = *new value*            | 改变 HTML 元素的属性值 |
| element.setAttribute(*attribute*, *value*) | 改变 HTML 元素的属性值 |
| element.style.property = *new style*       | 改变 HTML 元素的样式   |

## 添加和删除元素

| 方法                              | 描述             |
| --------------------------------- | ---------------- |
| document.createElement(*element*) | 创建 HTML 元素   |
| document.removeChild(*element*)   | 删除 HTML 元素   |
| document.appendChild(*element*)   | 添加 HTML 元素   |
| document.replaceChild(*element*)  | 替换 HTML 元素   |
| document.write(*text*)            | 写入 HTML 输出流 |

## 添加事件处理程序

| 方法                                                     | 描述                            |
| -------------------------------------------------------- | ------------------------------- |
| document.getElementById(id).onclick = function(){*code*} | 向 onclick 事件添加事件处理程序 |

## 查找 HTML 对象

首个 HTML DOM Level 1 (1998)，定义了 11 个 HTML 对象、对象集合和属性。它们在 HTML5 中仍然有效。

后来，在 HTML DOM Level 3，加入了更多对象、集合和属性。

| 属性                         | 描述                                        | DOM  |
| ---------------------------- | ------------------------------------------- | ---- |
| document.anchors             | 返回拥有 name 属性的所有 <a> 元素。         | 1    |
| document.applets             | 返回所有 <applet> 元素（HTML5 不建议使用）  | 1    |
| document.baseURI             | 返回文档的绝对基准 URI                      | 3    |
| document.body                | 返回 <body> 元素                            | 1    |
| document.cookie              | 返回文档的 cookie                           | 1    |
| document.doctype             | 返回文档的 doctype                          | 3    |
| document.documentElement     | 返回 <html> 元素                            | 3    |
| document.documentMode        | 返回浏览器使用的模式                        | 3    |
| document.documentURI         | 返回文档的 URI                              | 3    |
| document.domain              | 返回文档服务器的域名                        | 1    |
| document.domConfig           | 废弃。返回 DOM 配置                         | 3    |
| document.embeds              | 返回所有 <embed> 元素                       | 3    |
| document.forms               | 返回所有 <form> 元素                        | 1    |
| document.head                | 返回 <head> 元素                            | 3    |
| document.images              | 返回所有 <img> 元素                         | 1    |
| document.implementation      | 返回 DOM 实现                               | 3    |
| document.inputEncoding       | 返回文档的编码（字符集）                    | 3    |
| document.lastModified        | 返回文档更新的日期和时间                    | 3    |
| document.links               | 返回拥有 href 属性的所有 <area> 和 <a> 元素 | 1    |
| document.readyState          | 返回文档的（加载）状态                      | 3    |
| document.referrer            | 返回引用的 URI（链接文档）                  | 1    |
| document.scripts             | 返回所有 <script> 元素                      | 3    |
| document.strictErrorChecking | 返回是否强制执行错误检查                    | 3    |
| document.title               | 返回 <title> 元素                           | 1    |
| document.URL                 | 返回文档的完整 URL                          | 1    |



## 查找和访问 HTML 页面中的 HTML 元素

在 JavaScript 中查找和访问 HTML 页面中的 HTML 元素有多种方式，包括使用 `getElementById()`、`getElementsByClassName()`、`getElementsByTagName()`、`querySelector()` 和 `querySelectorAll()` 等方法。下面我将举例说明每种方法的用法：

1使用 `getElementById()` 获取具有特定 id 的元素：

```javascript
<!DOCTYPE html>
<html>
<head>
    <title>示例</title>
</head>
<body>
    <div id="myDiv">这是一个 div 元素</div>

    <script>
        var myElement = document.getElementById("myDiv");
        console.log(myElement.innerText);
    </script>
</body>
</html>

```

2.使用 `getElementsByClassName()` 获取具有特定 class 的元素：

```javascript
<!DOCTYPE html>
<html>
<head>
    <title>示例</title>
</head>
<body>
    <p class="myPara">第一个段落</p>
    <p class="myPara">第二个段落</p>

    <script>
        var paras = document.getElementsByClassName("myPara");
        for (var i = 0; i < paras.length; i++) {
            console.log(paras[i].innerText);
        }
    </script>
</body>
</html>

```

3.使用 `getElementsByTagName()` 获取具有特定标签名的元素：

```javascript
<!DOCTYPE html>
<html>
<head>
    <title>示例</title>
</head>
<body>
    <h1>标题</h1>
    <p>段落</p>

    <script>
        var headings = document.getElementsByTagName("h1");
        console.log(headings[0].innerText);
    </script>
</body>
</html>

```

4.使用 `querySelector()` 通过 CSS 选择器获取匹配的第一个元素：

```javascript
<!DOCTYPE html>
<html>
<head>
    <title>示例</title>
</head>
<body>
    <div class="container">
        <p>第一个段落</p>
        <p>第二个段落</p>
    </div>

    <script>
        var firstPara = document.querySelector(".container p");
        console.log(firstPara.innerText);
    </script>
</body>
</html>

```

5.使用 `querySelectorAll()` 通过 CSS 选择器获取匹配的所有元素：

```javascript
<!DOCTYPE html>
<html>
<head>
    <title>示例</title>
</head>
<body>
    <ul>
        <li>第一项</li>
        <li>第二项</li>
    </ul>

    <script>
        var listItems = document.querySelectorAll("ul li");
        listItems.forEach(function(item) {
            console.log(item.innerText);
        });
    </script>
</body>
</html>

```

## innerHTML 与innerText 区别

`innerHTML` 和 `innerText` 都是 JavaScript 中用于操作元素内容的属性，它们之间有一些区别：

1. `innerHTML`：
   - `innerHTML` 属性返回或设置元素内的 HTML 内容，包括标签和文本内容。
   - 当读取 `innerHTML` 属性时，它会返回元素内的 HTML 内容，包括标签。
   - 当设置 `innerHTML` 属性时，可以用字符串形式来设置元素的 HTML 内容，会直接替换原有内容。
   - `innerHTML` 具有解析 HTML 的能力，可以动态添加、修改和删除元素及其内容。
2. `innerText`：
   - `innerText` 属性返回或设置元素的文本内容，不包括 HTML 标签。
   - 当读取 `innerText` 属性时，它会返回元素内的文本内容，会自动忽略 HTML 标签。
   - 当设置 `innerText` 属性时，可以用字符串形式来设置元素的文本内容，会直接替换原有文本内容。
   - `innerText` 仅作用于文本内容，不会解析或执行其中的 HTML 标签。

```html
<!DOCTYPE html>
<html>
<head>
    <title>innerHTML 与 innerText 示例</title>
</head>
<body>
    <div id="myDiv">
        <p>This is <strong>bold</strong> text.</p>
    </div>

    <script>
        var myDiv = document.getElementById("myDiv");

        // 使用 innerHTML
        console.log("innerHTML 输出：");
        console.log(myDiv.innerHTML);

        // 使用 innerText
        console.log("innerText 输出：");
        console.log(myDiv.innerText);
    </script>
</body>
</html>

```

在这个示例中，`myDiv.innerHTML` 返回 `<p>This is <strong>bold</strong> text.</p>`，包括 HTML 标签和文本内容；而 `myDiv.innerText` 则返回 `This is bold text.`，只包括文本内容，HTML 标签被忽略。

根据需要选择使用 `innerHTML` 或 `innerText`，如果需要操作 HTML 结构并且需要处理 HTML 标签，可以使用 `innerHTML`；如果只需要处理纯文本内容，可以使用 `innerText`。



## JavaScript 可以改变css样式

```html
<!DOCTYPE html>
<html>
<head>
    <title>改变样式示例</title>
    <style>
        .highlight {
            color: red;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <p id="myParagraph">这是一个段落</p>
    <button onclick="changeStyle()">改变样式</button>

    <script>
        function changeStyle() {
            var paragraph = document.getElementById("myParagraph");
            paragraph.style.color = "blue";
            paragraph.style.fontWeight = "normal";
        }
    </script>
</body>
</html>

```

```javascript

```

