# JS 函数的增强知识

## 函数的属性

在 JS 中函数也是一个对象，对象中也可以有属性和方法

- `name`：函数的名字

  函数声明和函数表达式创建的函数都可以获取函数的名字

- `length`：函数参数（形参）的个数

  - rest 参数不参与 length 的计算
  
  - 有默认值的参数也不参与 length 的计算

## arguments 对象

arguments 是一个对应于 "传递给函数的参数"的类数组（array-like）对象

- 它拥有一些数组的特性

  比如有 length 属性 `arguments.length`，并且可以像数组一样通过索引访问 `arguments[0]`

- 但它没有数组的一些方法，比如 filter、map 等

将 arguments 转化成数组的方法

```js
// 1. 遍历放入新数组
var length = arguments.length
var argArr = []
for (var i = 0; i < length; i++) {
    arr.push(arguments[i])
}

// 2. 调用数组 slice 函数的 call 方法
var argArr1 = Array.prototype.slice.call(arguments)
var argArr2 = [].slice.call(arguments)

// 3. 用 ES6 中的方法
const argArr3 = Array.from(arguments)
const argArr4 = [...arguments]
```

箭头函数中没有 arguments 对象

所以在箭头函数中使用 arguments 会去上层作用域查找

## rest 参数

ES6 中引用了 rest parameter，可以将不定数量的参数放入到一个数组中

如果最后一个参数是 `...` 为前缀的，那么它会将剩余的参数放到该参数中，作为一个数组 

```js
function foo(num1, num2, ...otherNums) {
    console.log(otherNums) // [3, 4, 5]
}
foo(1, 2, 3, 4, 5)

function bar(...args) {
    console.log(args) // [a, b, c]
}
bar("a", "b", "c")
```

剩余参数和 arguments 的区别

- 剩余参数只包含那些没有对应形参的实参，而 arguments 包含了传给函数的所有实参
- arguments 是类数组对象，而 rest 参数是一个数组，可以进行数组的所有操作
- arguments 是早期 ECMAScript 为了方便获取所有的参数提取的一个数据结构，而 rest 参数是 ES6 中提供并且希望替代 arguments 的
- 剩余参数必须放到其他参数的最后

> 参数编写顺序
>
> 1. 无默认参数形参
> 2. 有默认参数形参
> 3. 剩余参数
>
> length 区分
>
> - fn.length：无默认参数形参个数
>
> - arguments.length：调用实参个数
>
> - rest.length：剩余参数个数

## 纯函数

函数式编程中有一个非常重要的概念叫纯函数

JavaScript 符合函数式编程的范式，所以也有纯函数的概念

纯函数的定义：如果一个函数符合以下条件，这个函数就是纯函数

- 函数在相同的输入值下，需要产生相同的输出
- 函数的输出与输入值以外的其他隐藏信息或状态无关，也和由 I/O 设备产生的外部输出无关
- 函数不能有语义上可观察的函数副作用，诸如触发事件，使输出设备输出，或更改输出值以外物件的内容等

==纯函数总结==

- 对于确定的输入一定会产生确定的输出（也就是输出只取决于函数的输入）

- 函数在执行过程中不能产生副作用

  副作用指的是，在执行一个函数时，除了返回函数值之外，还对外部产生了附加的影响

  比如修改了全局变量，修改了外部参数

纯函数的例子

`arr.slice()`：slice 在截取时会产生一个新的数组，不会修改原数组 

`arr.splice()`：splice 在截取时会返回一个新的数组，也会对原数组进行修改

slice 是纯函数，而 splice 不是纯函数

纯函数的作用和优势

- 纯函数在编写的时候只需要关注函数的逻辑，不需要关注传入参数是怎么获得的，或者传入参数依赖的外部变量是否已经发生了修改
- 纯函数在使用的时候，只需要确定传入的内容不会被篡改，对于确定的输入，就一定会有确定的输出

## 函数柯里化

**柯里化**（Currying），又译为卡瑞化或加里化

- 柯里化是把接收多个参数的函数，变成接受一个单一参数（最初函数的第一个参数）的函数，并且返回接受余下的参数而且返回结果的新函数的技术
- 简单的讲，只传递给函数一部分参数来调用它，让它返回一个函数去处理剩余参数的过程就叫做柯里化
- 柯里化是一种函数的转换，将一个函数从可调用的 `f(a, b, c)` 转换为 `f(a)(b)(c)`

```js
// 非柯里化写法
function add1(a, b, c) {
    return a + b + c
}

// 柯里化写法
function add2(a) {
    return function(b) {
        return function(c) {
            return a + b + c
        }
    }
}

// 箭头函数的写法
function adder3 = a => b => c => {
    return a + b + c
}
```

**柯里化的优势**

1. 函数的职责单一

   柯里化可以将一个函数中的一系列复杂操作转化成一系列函数的连续操作

   可以将每次传入的参数在单一的函数中进行处理，处理后在下一个函数中使用处理后的结果

   ```js
   function add2(a) {
       a = a + 2
       return function(b) {
           b = b + 2
           return function(c) {
               c = c + 2
               return a + b + c
           }
       }
   }
   ```

2. 有利于函数的逻辑复用

   ```js
   function createAdder(count) {
       return function(num) {
           return count + num
       }
   }
   
   var add5 = makeAdder(5)
   add5(10)
   add5(20)
   
   var add10 = makeAdder(10)
   add10(10)
   add10(20)
   ```

**柯里化案例**：打印日志函数

```js
var log = date => type => message => {
    console.log(`[${date.getHours()}:${date.getMinutes()}] [${type}] [${message}]`)
}

var logNow = log(new Date())
logNow("DEBUG")("Some issues went wrong")

var logNowDebug = log(new Date())("DEBUG")
logNowDebug("Some issues went wrong")
```

**自动柯里化函数**

```js
function add(a, b, c) {
    return a + b + c
}

function autoCurrying(fn) {
    function curryFn(...args) {
        if (args.length >= fn.length) {
            // return fn(...args)
            return fn.apply(this, args)
        } else {
            return function(...newArgs) {
                // return curryFn.apply(...args.concat(newArgs))
                return curryFn.apply(this, args.concat(newArgs))
            }
        }
    }
    return curryFn
}

var addCurry = autoCurrying(add)
addCurry(10)(20)(30)
```

分析：

addCurry(10) 执行 curryFn(10) 得到

```js
// args = [10]
function(...newArgs) {
    return curryFn.apply(this, args.concat(newArgs))
}
```

addCurry(10)(20) 会执行上面函数最终执行 curryFn(10, 20) 得到

```js
// args = [10, 20]
function(...newArgs) {
    return curryFn.apply(this, args.concat(newArgs))
}
```

addCurry(10)(20)(30) 会执行上面函数最终执行 curryFn(10, 20, 30) 继而执行 fn(10, 20, 30)

自动柯里化函数的弊端

- 只是调用方式变得柯里化了。前期利用闭包收集参数，在最后一步执行原函数。
- 真正的柯里化函数，是可以逻辑复用的。即逻辑复用那部分可以不需要重复调用，直接使用第一次调用的结果即可。
- 自动柯里化函数只是形式上的柯里化

## 组合函数

组合（Compose）函数是在 JavaScript 开发过程中一种对函数的使用技巧、模式

- 组合函数就是将函数组合在一起，自动依次调用
- 适用于多个函数的组合重复调用

两个函数的组合

```js
function double(num) {
    return num * 2
}

function pow(num) {
    return num ** 2
}

function composeFn(num) {
    return pow(double(num))
}
```

自动组合多个函数

```js
function compose(...fns) {
    if (fns.length <=0) return
    for (var fn of fns) {
        if (typeof fn !== "function") {
            throw new TypeError(`index position ${i} must be function`)
        }
    }
    
    return function(...args) {
        var result = fns[0].apply(this, args)
        for (var i = 1; i < fns.length; i++) {
            result = fns[i].apply(this, [result])
        }
        return result
    }
}

var newFn = compose(double, pow)
var result = newFn(100)
```

# JS 对象的增强知识

## 属性描述符

**属性描述符**用于对一个属性的精准操作控制

- 属性描述符可以精准的添加或修改对象的属性

- 属性描述符需要使用 `Object.defineProperty()` 来对属性进行添加或者修改

  `Object.defineProperty(obj, prop, descriptor)`

  这个方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回此对象

  - obj：要定义属性的对象
  - prop：要定义或修改的属性的名称或 Symbol
  - descriptor：要定义或修改的属性描述符

**属性描述符的分类**

1. 数据属性描述符（Data Properties Descriptor）

   - `[[Configurable]]`：表示属性是否可以通过 delete 删除属性，是否可以修改它的特性（属性描述符），或者是否可以将它修改为存取属性描述符
     - 当我们直接在一个对象上定义某个属性时，这个属性的 `[[Configurable]]` 为 true
     - 当我们通过属性描述符定义一个属性时，这个属性的 `[[Configurable]]` 默认为 false
   - `[[Enumerable]]`：表示属性是否可以通过 `for-in` 或者 `Object.keys()` 返回该属性
     - 当我们直接在一个对象上定义某个属性时，这个属性的 `[[Enumerable]]` 为 true
     - 当我们通过属性描述符定义一个属性时，这个属性的 `[[Enumerable]]` 默认为 false
   - `[[Writable]]`：表示是否可以修改属性的值
     - 当我们直接在一个对象上定义某个属性时，这个属性的 `[[Writable]]` 为 true
     - 当我们通过属性描述符定义一个属性时，这个属性的 `[[Writable]]` 默认为 false
   - `[[value]]`：属性的 value 值，读取属性时会返回该值，修改属性时，会对其进行修改
     - 默认情况下这个值是 undefined

2. 存取属性描述符（Accessor Properties Descriptor）

   - `[[Configurable]]`：和数据属性描述符一致
   - `[[Enumerable]]`：和数据属性描述符一致
   - `[[get]]`：获取属性时会执行的函数，默认为 undefined
   - `[[set]]`：设置属性时会执行的函数，默认为 undefined

   ```js
   var obj = {
     name: "you",
     age: 18
   }
   
   var _name = "me"
   Object.defineProperty(obj, "name", {
     configurable: true,
     enumerable: false,
     set: function(value) {
       _name = value
     },
     get: function() {
       return _name
     }
   })
   
   ```

`Object.defineProperties()` 方法会直接在一个对象上定义多个新的属性或修改现有属性，并且返回该对象

```js
var obj = {
  _age: 18
}

Object.defineProperties(obj, {
  name: {
    writable: true,
    value: "me"
  },
  age: {
    get: function() {
      return this._age
    }
  }
})
```

## 常见实例方法

- `obj.hasOwnProperty(prop)`：判断对象是否有某一个属于自己的属性：

- `prop in obj`：判断对象或者其原型上是否有某一个属性

- `for (var key in obj) {}`：遍历对象或者其原型上的所有可枚举属性

  for in 遍历的属性不包含 symbol 属性和不可枚举shu'xing


>typeof 操作符可以用于确定变量的数据类型
>
>对一个值使用 typeof 操作符会返回下列字符串之一
>
>- `number`：表示值为数值
>- `string`：表示值为字符串
>- `boolean`：表示值为布尔值
>- `undefined`：表示值未定义
>- `object`：表示值为对象（而不是函数）或 null
>- `function`：表示值为函数
>- `bigint`：表示值为大整数
>- `symbol`：表示值为符号

- `instanceof`：判断对象和类之间的关系

  用于检测构造函数的原型对象是否出现在某个实例对象的原型链上

  ```js
  var stu = new Student()
  stu instanceof Student // true
  stu instanceof Person // true
  stu instanceof Teacher // false
  ```

- `obj1.isPrototypeOf(obj2)`：判断两个对象之间的继承关系

  用于检测某个对象是否出现在某个实例对象的原型链上

  ```js
  Student.prototype.isPrototypeOf(stu) // true
  Person.prototype.isPrototypeOf(stu) // true
  Teacher.prototype.isPrototypeOf(stu) // false
  
  var obj = {}
  var info = Object.create(obj)
  obj.isPrototypeOf(info) // true
  ```

## 常见类方法

- `Object.hasOwn(obj, propKey)`：判断一个对象中是否有某个属性
- `Object.keys(obj)`：获取对象属性名，不包含 symbol 属性 和 enumerable 为 false 的属性
- `Object.getOwnPropertyNames`：获取对象属性名，包含所有非 symbol 属性
- `Object.getOwnPropertySymbols(obj)`：获取对象属性名，只有 symbol 属性

- `Object.getPrototypeOf(obj)`：返回指定对象的原型

- `Object.setPrototypeOf(obj, prototype)`

  将一个指定对象的原型（即内部的 `[[Prototype]]` 属性）设置为另一个对象或者 null

- `Object.create(obj)`：以一个现有对象作为原型，创建一个新对象

- `Object.getOwnPropertyDescriptor(obj, prop)`：获取对象的属性描述符

- `Object.getOwnPropertyDescriptors(obj)`：获取对象的多个属性的属性描述符

- `Object.preventExtensions(obj)`：禁止对象扩展新属性

- `Object.seal(obj)`：密封对象，不允许扩展、配置和删除属性
  
  - 实际是调用 `preventExtensions`
  - 并且将现有属性的 configurable 设置为 false
  
- `Object.freeze(obj)`：冻结对象，不允许修改现有属性
  
  - 实际上是调用 seal
  - 并且将现有属性的 writable 设置为 false

- `Object.values(obj)`：获取所有的 value 值

  传入字符串可以获得包含每个字符的数组

- `Object.entries(obj)`：获取存放可枚举属性的键值对数组

  可以用于对象、数组、字符串

- `Object.fromEntries()`：可以将 entries 转换成对象

## 增强对象字面量

ES6 中对对象字面量进行了增强，称之为 Enhanced object literals 增强对象字面量

- 属性的简写：Property Shorthand

  ```js
  var name = "me"
  var age = 18
  var obj = {
    name, // 简写前：name: name
    age // 简写前：age: age
  }
  ```

- 方法的简写：Method Shorthand

  ```js
  var obj = {
    name, // 简写前：name: name
    age // 简写前：age: age
    // 简写前
    running: function() {
  
    },
    // 简写方法
    sleeping() {
  
    }
    eating: () => {
  
    }  
  }
  ```

- 计算属性名：Computed Property Names

  ```js
  var key = "address"
  var obj = {
      key: "hangzhou",
      [key]: "zhengzhou"
  }
  obj.key // hangzhou
  obj.address // zhengzhou
  ```

## 解构

ES6 中新增了一个从数组或对象中方便获取数据的方法，称之为解构 Destructuring

解构是一种特殊的赋值语法，它使我们可以将数组或对象 “拆包” 至一系列变量中

解构常用于函数的参数和返回值

### 数组的解构

```js
var names = ["name1", "name2", "name3", "name4", undefined]

// 基本使用
var [name1, name2, name3, name4] = names
// 数组的解构有严格的顺序要求
var [name1, , name3] = names
// 解构成数组
var [name1, name2, ...otherNames] = names
// 解构的默认值
var [name1, name2, name3, name4, name5 = "default"] = names
```

### 对象的解构

```js
var obj = {
    name: "me",
    age: 18,
    height: 1.88,
    address: "zhengzhou"
}

// 基本使用
var { name, age, height } = obj
// 对象的解构没有顺序，是根据属性的 key 解构的
var { height, name, age } = obj
// 变量重命名
var { name, age, height, address: schoolAddress } = obj
// 解构的默认值
var { name, age, height, address: schoolAddress = "china", sex = "male" } = obj
// 剩余变量可以解构在一个对象里
var { name, age, ...rest } = obj
```

## 深拷贝

深拷贝、浅拷贝和引用赋值的对比

```js
const info = {
    name: "me",
    friend: {
        name: "you"
    }
}

// 引用赋值
const obj1 = info // obj1 和 info 保存的是同一个内存地址

// 浅拷贝
const obj2 = {
    ...info,
    age: 20
}
obj2.friend.name = "kobe" // info 中的也会发生改变
// Object.assign(target, ...sources) 会将一个或多个源对象中的属性浅拷贝到目标对象

// 深拷贝：使用 JSON 或者 使用深拷贝函数
const obj3 = JSON.parse(JSON.stringfy(info))

// lodash 的深拷贝函数
const obj4 = _.cloneDeep(info)
```

# 其他增强知识

## with 语句

with语句可以扩展一个语句的作用域链

不建议使用with语句，因为它可能是混淆错误和兼容性问题的根源

```js
var obj = {
    message: "Hello"
}
with (obj) {
    console.log(message) // 先在 obj 中查找，找不到再去全局作用域中找
}
```

## eval 函数

内置函数 eval 允许执行一个代码字符串

- eval 是一个特殊的函数，它可以将传入的字符串当做 JavaScript 代码来运行
- eval 会将最后一句执行语句的结果，作为返回值
- eval 函数的字符串中可以获取全局变量

```js
var evalString = `var message = "Hello"; console.log(message);`
eval(evalString)
```

但并不建议使用 eval 函数执行代码

- eval 代码的可读性非常的差（代码的可读性是高质量代码的重要原则）
- eval 是一个字符串，那么有可能在执行的过程中被刻意篡改，那么可能会造成被攻击的风险
- eval 的执行必须经过 JavaScript 解释器，不能被 JavaScript 引擎优化

## 严格模式

JS 的历史局限性

- 长久以来，JavaScript 不断向前发展且并未带来任何兼容性问题
- 新的特性被加入，旧的功能也没有改变，这么做有利于兼容旧代码
- 但缺点是 JavaScript 创造者的任何错误或不完善的决定也将永远被保留在 JavaScript 语言中

ECMAScript5 中 JS 提出了严格模式（Strict Mode）的概念

- 严格模式是一种具有限制性的 JavaScript 模式，使代码隐式的脱离了懒散模式（Sloppy Mode）

- 支持严格模式的浏览器在检测到代码中有严格模式时，会以更加严格的方式对代码进行检测和执行
- 严格模式对正常的 JavaScript 语义进行了一些限制
  1. 严格模式通过抛出错误来消除一些原有的静默（silent）错误
  2. 严格模式让 JS 引擎在执行代码时可以进行更多的优化（不需要对一些特殊的语法进行处理）
  3. 严格模式禁用了在 ECMAScript 未来版本中可能会定义的一些语法

- 严格模式通过在文件或者函数开头使用 `"use strict"` 来开启
- 现代 JavaScript 支持 class 和 module，它们会自动启用严格模式

严格模式的常见限制

- 不会意外地创建全局变量

- 严格模式会使引起静默失败（silently fail 即不报错也没有任何效果）的赋值操作抛出异常

  比如严格模式下试图删除不可删除的属性

- 严格模式不允许函数参数有相同的名称

- 不允许 0 的八进制语法，可以使用 `0o`

- 在严格模式下，不允许使用 with 语句

- 在严格模式下，eval 不能为上层作用域创建变量

- 严格模式下，this绑定不会默认转成对象类型

- 独立函数执行默认模式下，this 会绑定 window 对象，而严格模式下是 undefined

## 错误处理

### throw

在函数中想告知外界自己内部出现了错误可以通过 throw 关键字抛出一个异常

throw 语句

- throw 语句用于抛出一个用户自定义的异常
- 当遇到 throw 语句时，当前的函数执行会被停止（ throw 后面的语句不会执行）

- throw 语句就是 `throw expression` 即在 throw 后面可以跟上一个表达式来表示具体的异常信息
  - 表达式可以是基本数据类型：比如Number、String、Boolean
  - 也可以是对象类型：对象类型可以包含更多的信息
  - 可以使用内置的 Error 类创建的对象

### Error

JS 中提供了一个 Error 类

Error 类有三个属性

- message：创建 Error 对象时传入的 message
- name：Error 的名称，通常和类的名称一致
- stack：整个 Error 的错误信息，包括函数的调用栈，当我们直接打印 Error 对象时，打印的就是 stack

Error 类的子类

- RangeError：下标值越界时使用的错误类型
- SyntaxError：解析语法错误时使用的错误类型
- TypeError：出现类型错误时，使用的错误类型

### try catch

函数抛出异常时，如果没有被捕获处理，程序会被终止

- 如果我们在调用一个函数时，这个函数抛出了异常，但是我们并没有对这个异常进行处理，那么这个异常会继续传
  递到上一个函数调用中
- 而如果到了最顶层（全局）的代码中依然没有对这个异常的处理代码，这个时候就会报错并且终止程序的运行

但是很多情况下当出现异常时，我们并不希望程序直接退出，而是希望可以正确的处理异常

这个时候可以使用 try catch 捕获异常

```js
try {
    try_statements
}
[catch(exception_var_1) {
    catch_statements_1  
}]
[finally {
    finally_statements
}]
```

ES10 中 catch 后面绑定的 error 可以省略

如果有一些必须要执行的代码，我们可以使用 finally 来执行

- finally 表示最终一定会被执行的代码结构
- 如果 try 和 finally 中都有返回值，那么会使用 finally 当中的返回值
