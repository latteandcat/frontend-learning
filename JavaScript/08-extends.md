# 原型

##  对象的原型

在 JS 中，每个对象都有一个特殊的内置属性 `[[Prototype]]`

这个特殊的属性指向的另外一个对象就是对象的原型对象

原型对象的获取方式

- 可以通过 `obj.__proto__` 获取（浏览器添加的，有兼容性问题）
- 也可以通过 `Object.getPrototypeOf(obj)` 获取

原型对象的作用

1. 当我们通过引用对象的属性 key 来获取一个 value 时，它会触发 `[[Get]]` 的操作
2. 这个操作会首先检查该对象是否有对应的属性，如果有的话就使用它
3. 如果对象中没有该属性，那么会访问对象 `[[prototype]]` 内置属性指向的对象上的属性

##  函数的原型

`prototype` 属性

所有的函数（非箭头函数）都有一个 `prototype` 属性，可以通过 `fn.prototype` 获取

函数的 `prototype` 属性的作用是，在函数在作为构造函数用于构建对象时，构造函数会将自身的 `prototype` 属性（显式原型）赋值给创建出来的对象的 `[[prototype]]` 属性（隐式原型）

```js
function Foo() {
    
}
var f1 = new Foo()
var f2 = new Foo()
// f1.__proto__ === f2.__proto__ === Foo.prototype 
```

应用场景：

当多个对象拥有共同的属性或方法时，我们可以将它放在构造函数的 `prototype` 属性中

由构造函数创建出来的所有对象，都会共享这些值

注意：如果在创建的实例对象上修改原型对象上存在的属性，会在这个对象上添加这个属性，而不是修改原型对象上属性的值

```js
Person.prototype.address = "zh"

p1 = new Person()
p2 = new Person()
console.log(p1.address) // "zh"
console.log(p2.address) // "zh"
p1.address = "zh-hn"
console.log(p1.address) // "zh-cn"
console.log(p2.address) // "zh"
```

如果我们需要在原型上添加过多的属性，通常我们会重写整个原型对象

```js
function Person() {
    
}

Person.prototype = {
    name: "me",
    age: 18,
    height: 1.88,
    // constructor: Person
}

Object.defineProperty(Person.prototype, "constructor", {
    enumerable: false,
    configurable: true,
    writable: true,
    value: Person
})
```

重写整个原型对象相当于给 prototype 重新赋值了一个对象

这个新对象的 constructor 属性会指向 Object 构造函数，而不是 Person 构造函数

所以需要手动指定新对象的 constructor 属性

但是 constructor 属性本来是不可枚举的，这样指定的 constructor 属性是可枚举的

`Object.defineProperty()` 可以解决这个问题

## constructor

原型对象上有一个 `constructor` 属性

默认情况下原型上都会添加这个属性，并指向对应的构造函数对象

```js
function Person() {
    
}

Person.prototype.constructor === Person //true

var p = new Person()
p.__proto__.constructor === Person //true
```

# ES5  继承

## 面向对象的te'xi

面向对象有三大特性：封装、继承、多态

- 封装：将属性和方法封装到一个类中，可以称之为封装的过程

- 继承：继承是面向对象中非常重要的，不仅仅可以减少重复代码的数量，也是多态的前提（纯面向对象中）

  继承的作用

  - 继承可以帮助我们将重复的代码和逻辑抽取到父类中，子类只需要直接继承过来使用即可
  - 继承是多态的前提

- 多态：不同的对象在执行时表现出不同的形态

  - 多态（polymorphism）指为不同数据类型的实体提供统一的接口，或使用一个单一的符号
    来表示多个不同的类型

  - 简单来说，不同的数据类型进行同一个操作，表现出不同的行为，就是多态的体现

    比如矩形和圆形都是图形，都可以计算面积，但它们计算面积的方式不同


## 原型链

从一个对象上获取属性，如果在当前对象中没有获取到就会去它的原型对象上面获取

如果原型对象上也没有就沿着原型链去原型对象的原型对象上获取

最终找到 `Object.prototype`，`Object.prototype.__proto__` 为 null

原型链最顶层的原型对象就是 Object 的原型对象

## 原型链继承

1. 定义父类构造函数
2. 在父类构造函数的原型对象上添加内容
3. 定义子类构造函数
4. 创建父类实例对象，并且作为子类构造函数的原型对象
5. 在子类构造函数的原型对象上添加内容

```js
// 定义父类构造函数
function Person(name, age) {
    this.name = name
    this.age = age
}

// 在父类原型上添加内容
Person.prototype.running = function() {
    
}
Person.prototype.eating = function() {
    
}

// 定义子类构造函数
function Student(sno, name, age) {
    this.name = name
    this.age = age
    this.sno = sno
}

// 如果直接让 Student.prototype = Person.prototype 可以让子类调用父类原型上的方法，但是后续在在子类原型上添加内容时，父类原型上也会添加，让父类的实例对象作为子类的原型对象可以解决这个问题

// 创建父类实例对象，并且作为子类的原型对象
var p = new Person("me", 18)
Student.prototype = p

// 在子类原型上添加内容
Student.prototype.studying = function() {
    
}
```

原型链继承的弊端

如果不在子类构造函数中重复声明父类属性就会出现以下问题

- 直接打印对象看不到继承的属性
- 继承的属性会被多个对象共享，如果这个对象是引用类型就会造成问题
- 不能给父类构造函数传递参数，因为这个对象是一次性创建的

## 借用构造函数继承

借用构造函数 constructor stealing 也可以叫做经典继承或伪造对象

做法就是在子类构造函数的内部调用父类构造函数

- 因为函数可以在任意的时刻被调用
-  因此通过 `apply()` 和 `call()` 方法也可以在新创建的对象上执行构造函数

```js
function Student(sno, name, age) {
    // this.name = name
    // this.age = age
    Person.call(this, name, age)
    this.sno = sno
}
```

借用构造函数可以解决原型链继承中的一些弊端

借用构造函数和原型链结合也叫做**组合继承**，是 ES5 中最常用的继承模式之一

但组合继承也有一些问题

- 会调用两次父类构造函数

  第一次是创建子类构造函数原型对象的时候

  第二次是在子类构造函数内部借用构造函数的时候

- 所有的子类实例会拥有两份父类的属性

  第一份是借用构造函数创建的属于自己的父类属性

  第二份是在子类的原型对象中的父类属性

## 原型式继承函数

原型链继承中第四步的根本目的

- 创建出来一个对象
- 这个对象的隐式原型指向父类的显式原型
- 将这个对象赋值给子类的显式原型

```js
// 原型链继承中的做法, 存在一些弊端
var p = new Person("me", 18)
Student.prototype = p

// 方案一：使用新对象
var obj = {}
Object.setPrototypeOf(obj, Person.prototype)
Student.prototype = obj

// 方案二：使用新构造函数的实例对象
function F() {}
F.prototype = Person.prototype
Student.prototype = new F()

// 方案三：使用 Object.create()
var obj = Object.create(Person.prototype)
Student.prototype = obj
```

将以上三种方案封装成函数就叫做原型式继承函数

作用是返回一个继承自传入对象的新对象

```js
function createObject(obj) {
    var newObj = {}
    Object.setPrototypeOf(newObj, obj)
    return newObj
}

function createObject(obj) {
    function F() {}
    F.prototype = obj
    return new F()
}

function createObject(obj) {
    return Object.create(obj)
}
```

## 寄生式继承函数

寄生式继承的思路是结合原型式继承和工厂模式

创建一个封装继承过程的函数，该函数在内部以某种方式增强对象，最后将这个对象返回

```js
function createObject(obj) {
    function F() {}
    F.prototype = obj
    return new F()
}

function createStudent(person, sno) {
    var stu = createObject(person)
    stu.sno = sno
    stu.studying = function() {
        
    }
    return stu
}
```

## 寄生组合式继承

```js
function createObject(obj) {
    function F() {}
    F.prototype = obj
    return new F()
}

function inherit(subType, superType) {
    subType.prototype = createObject(superType.prototype)
    Object.defineProperty(subType.prototype, "constructor", {
        enumerable: false,
        configurable: true,
        writable: true,
        value: subType
    })
    Object.setPrototypeOf(subType, superType) // 保证类方法的继承
}
```

将寄生式继承和组合继承结合起来可以解决组合继承中的弊端

这种方案就叫做寄生组合式继承

```js
function Person(name, age) {
    this.name = name
    this.age = age
}

Person.prototype.running = function() {
    
}

Person.prototype.eating = function() {
    
}

function Student(sno, name, age) {
    Person.call(this, name, age)
    this.sno = sno
}

inherit(Student, Person)

Student.prototype.studying = function() {
    
}
```

# ES6 继承

## class 类

ES6 之前只能使用构造函数来创建类，但可读性较差，不容易理解

ES6 中使用了 class 关键字来直接定义类

### 类的定义

定义类的两种方式

```js
class Person {
    
}

var Student = class {
    
}
```

类只能通过 new 操作符调用

### 类的构造函数

使用 class 定义类时可以通过类里面的 constructor 构造函数传递参数

- 通过 new 操作符调用类的时候会调用 constructor 构造函数，并且执行如下操作
  1. 在内存中创建一个新的对象（空对象）
  2. 这个对象内部的 `[[prototype]]` 属性会被赋值为该类的 prototype 属性
  3. 构造函数内部的 this，会指向创建出来的新对象
  4. 执行构造函数的内部代码
  5. 如果构造函数没有返回非空对象，则返回创建出来的新对象
- 每个类只能有一个 constructor 函数

```js
class Person {
    constructor(name, age) {
        this.name = name
        this.age = age
    }
}

var p1 = new Person("me", 18)
Person.prototype === p1.__proto__ // true
```

### 类的实例方法

实例方法也可以直接在类里面定义，这些方法会放在类的原型对象上供实例对象使用 

```js
class Person {
    constructor(name, age) {
        this.name = name
        this.age = age
    }
    
    runnning() {
        console.log(this.name + "is running")
    }
    
    eating() {
        console.log(this.name + "is eating")
    }
}
```

注意：类中定义的多个内容不需要使用，进行分割

### 类的访问器方法

```js
class Person {
    constructor(name) {
        this._name = name
    }
    
    set name(newName) {
        this._name = newName
    }
    
    get name() {
        return this._name
    }
}
```

访问器的应用场景

```js
class Rectangle {
    constructor(x, y, width, height) {
        this.x = x
        this.y = y
        this.width = width
        this.height = height
    }
    
    get position() {
        return { x: this.x, y: this.y }
    }
    
    get size() {
        return { width: this.width, height: this.height }
    }
}

var rect1 = new Rectangle(10, 20, 100, 200)
console.log(rect1.position)
console.log(rect1.size)
```

### 类的静态方法

静态方法通常用于定义直接使用类来执行的方法，不需要有类的实例，使用 static 关键字来定义

```js
class Person {
    constructor(name, age) {
        this.name = name
        this.age = age
    }
    
    static createPerson() {
        return new this("", 0)
    }
}

var p = Person.createPerson()
```

## 类的继承

在 ES6 中新增了使用 extends 关键字，可以方便的帮助我们实现继承

```js
class Person {
    constructor(name, age) {
        this.name = name
        this.age = age
    }
    runnning() {
        console.log(this.name + "is running")
    }
    eating() {
        console.log(this.name + "is eating")
    }
}

class Student extends Person {
    constructor(name, age, sno) {
        super(name, age)
        this.sno = sno
    }
    studying() {
        console.log(this.name + "is studying")
    }
    // 父类方法的重写
    running() {
        super.running()
    }
}
```

class 提供了 super 关键字来调用父类的 constructor 和父类的方法

- `super.method(args)`：调用一个父类方法

  可以调用实例方法，也可以调用静态方法

- `super(args)`：调用父类 constructor（只能在子类的 constructor 里调用）

  在子类的构造函数中使用 this 或者返回默认对象之前，必须先通过 super 调用父类的构造函数

  最好放在子类构造函数的最前面

通过继承内置类可以对内置类进行一些扩展

```js
class myArray extends Array {
    get lastItem() {
        return this[this.length - 1]
    }
    get firstItem() {
        return this[0]
    }
}
```

## 类的混入

JavaScript 的类只支持单继承，也就是只能有一个父类

混入就是通过函数传入一个类，返回一个继承自传入类的类

```js
function mixinRunner(BaseClass) {
    return class extends BaseClass {
        running() {
            console.log("running")
        }
    }
}

function mixinEater(BaseClass) {
    return class extends BaseClass {
        eating() {
            console.log("eating")
        }
    }
}

class Person {}

class NewPerson extends mixinEater(mixinRunner(Person)) {}

var np = new NewPerson()
np.eating()
np.running()
```

