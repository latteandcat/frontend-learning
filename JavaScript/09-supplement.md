# Proxy

## 监听对象的操作

监听一个对象中属性被设置或获取的过程

```js
Object.keys(obj).forEach(key => {
    let value = obj[key]
    Object.defineProperty(obj, key, {
        set: function(newValue) {
            value = newValue
            console.log(`监听到给${key}设置值`)
        }，
        get: function() {
            return value
            console.log(`监听到获取${key}的值`)
        }
    })
})
```

弊端：

- Object.defineProperty 设计的初衷，不是为了去监听截止一个对象中所有的属性的

- 我们在定义某些属性的时候，初衷其实是定义普通的属性，但是后面我们强行将它变成了数据属性描述符
- 如果我们想监听更加丰富的操作，比如新增属性、删除属性，那么 Object.defineProperty 是无能为力的

## Proxy 基本使用

在ES6中，新增了一个Proxy类，用于帮助我们创建一个代理

如果我们希望监听一个对象的相关操作，那么我们可以先创建一个代理对象（Proxy对象）

之后对该对象的所有操作，都通过代理对象来完成，代理对象可以监听我们想要对原对象进行哪些操作

创建 Proxy 对象：`const p = new Proxy(target, handler)`

```js
const obj = {
    name: "me",
    age: 19
}

// 创建 proxy 对象
const objProxy = new Proxy(obj, {})

// 后续所有对 obj 的操作都转化为对 objProxy 的操作
```

## Proxy 捕获器

如果我们想要侦听某些具体的操作，那么就可以在 handler 中添加对应的捕捉器（Trap）

set 和 get 捕获器对应的是函数类型

set 函数有四个参数

- target：目标对象，即监听的对象
- property：将被设置的属性 key
- value：新属性值
- receiver：调用的代理对象

get 函数有四个参数

- target：目标对象
- property：被获取的属性 key
- receiver：调用的代理对象

```js
const objProxy = new Proxy(obj, {
    has: function(target, key) {
      return key in target  
    },
    set: function(target, key, value) {
        target[key] = value
        console.log(`监听到给${key}设置值`)
    },
    get: function(target, key) {
        return target[key]
        console.log(`监听到获取${key}的值`)
    },
    deleteProperty: function(target, key) {
        delete target[key]
    }
})
```

## 所有捕获器

- `handler.getPrototypeOf()`：Object.getPrototypeOf 方法的捕捉器
- `handler.setPrototypeOf()`：Object.setPrototypeOf 方法的捕捉器
- `handler.isExtensible()`：Object.isExtensible 方法的捕捉器（判断是否可以新增属性）
- `handler.preventExtensions()`：Object.preventExtensions 方法的捕捉器
- `handler.getOwnPropertyDescriptor()`：Object.getOwnPropertyDescriptor 方法的捕捉器
- `handler.defineProperty()`：Object.defineProperty 方法的捕捉器
- `handler.ownKeys()`：Object.getOwnPropertyNames 方法和 Object.getOwnPropertySymbols 方法的捕捉器
- `handler.has()`：in 操作符的捕捉器
- `handler.get()`：属性读取操作的捕捉器
- `handler.set()`：属性设置操作的捕捉器
- `handler.deleteProperty()`：delete 操作符的捕捉器
- `handler.apply()`：函数调用操作的捕捉器
- `handler.construct()`：new 操作符的捕捉器

## 函数对象捕获器

construct 和 apply 是应用于函数对象的

```js
function foo() {
    console.log("foo")
}

const fooProxy = new Proxy(foo, {
    apply: function(target, thisArg, otherArgs) {
        return target.apply(thisArg, otherArgs)
    },
    construct: function(target, argArray, newTarget) {
        return new target()
    }
})
```

# Reflect

## Reflect 的作用

Reflect 也是 ES6 新增的一个 API，它是一个对象，字面的意思是反射

Reflect 提供了很多操作 JavaScript 对象的方法，有点像 Object 中操作对象的方法

>**Reflect 出现的历史原因**
>
>早期的 ECMA 规范中没有考虑到这种对 **对象本身** 的操作如何设计会更加规范，所以将这些API放到了 Object 上面
>
>但是 Object 作为一个构造函数，这些操作实际上放到它身上并不合适
>
>另外还包含一些类似于 in、delete 操作符，让 JS 看起来是有一些奇怪
>
>所以在 ES6 中新增了 Reflect，让我们这些操作都集中到了 Reflect 对象上

使用 Reflect API 的好处

- 可以不再直接操作原对象，更符合代理对象的初衷

- 有返回值，可以知道操作是否成功
- 可以设置 receiver，receiver 就是外层的代理对象

## Reflect 的常见方法

- `Reflect.getPrototypeOf(target)`

  类似于 Object.getPrototypeOf()

- `Reflect.setPrototypeOf(target, prototype)`

  设置对象原型的函数，返回一个 Boolean，如果更新成功，则返回 true

- `Reflect.isExtensible(target)`

  类似于 Object.isExtensible()

- `Reflect.preventExtensions(target)`

  类似于 Object.preventExtensions()，返回一个Boolean

- `Reflect.getOwnPropertyDescriptor(target, propertyKey)`

  类似于 Object.getOwnPropertyDescriptor()，如果对象中存在该属性，则返回对应的属性描述符，否则返回 undefined

- `Reflect.defineProperty(target, propertyKey, attributes)`

  和 Object.defineProperty() 类似，如果设置成功就会返回 true

- `Reflect.ownKeys(target)`

  返回一个包含所有自身属性（不包含继承属性）的数组，类似于 Object.keys()，但不会受 enumerable 影响

  for in 和 Object.keys() 都会受 enumerable 影响

- `Reflect.has(target, propertyKey)`

  判断一个对象是否存在某个属性，和 in 运算符的功能完全相同

- `Reflect.get(target, propertyKey[, receiver])`

  获取对象身上某个属性的值，类似于 target[name]

- `Reflect.set(target, propertyKey, value[, receiver])`

  将值分配给属性的函数，返回一个Boolean，如果更新成功，则返回true

- `Reflect.deleteProperty(target, propertyKey)`

  作为函数的 delete 操作符，相当于执行 delete target[name]

- `Reflect.apply(target, thisArgument, argumentsList)`

  对一个函数进行调用操作，同时可以传入一个数组作为调用参数，和 Function.prototype.apply() 功能类似

- `Reflect.construct(target, argumentsList[, newTarget])`

  对构造函数进行 new 操作，相当于执行 new target(...args)

## Reflect 的使用

可以将之前 Proxy 案例中对原对象的操作，都修改为 Reflect 来操作

```js
const objProxy = new Proxy(obj, {
    has: function(target, key) {
        // return key in target  
        return Reflect.has(target, key)
    },
    set: function(target, key, value) {
        // target[key] = value
        Reflect.set(target, key, value)
    },
    get: function(target, key) {
        // return target[key]
        Reflect.get(target, key)
    },
    deleteProperty: function(target, key) {
        // delete target[key]
        Reflect.deleteProperty((target, key))
    }
})
```

## Receiver的作用

如果源对象有 set 和 get 的访问器方法，那么可以通过 receiver 来改变里面的 this

传入 receiver 就可以监听到访问器方法内部的对象操作

```js
const obj = {
    _name = "me",
    set name(value) {
        this._name = value
    }
    get name() {
        return this._name
    }
}

const objProxy = new Proxy(obj, {
    set: function(target, key, newValue, receiver) {
        console.log("set property")
        Reflect.set(target, key, newValue, receiver)
    },
    get: function(target, key, receiver) {
        console.log("get property")
        return Reflect.get(target, key, receiver)
    }
})
```

## Reflect 的 construct

Reflect.construct 可以用于借用构造函数

```js
function Person(name, age) {
    this.name = name
    this.age = age
}

function Student() {
    // Person.call(this, name, age)
}

const stu = Reflect.construct(Person, ["me", 18], Student)
console.log(stu)
console.log(stu.__proto__ === Student.prototype) // true
```

# Promise

## 异步任务

简单异步任务案例

1. 首先调用一个函数，这个函数中会发送网络请求
2. 如果发送网络请求成功了，那么告知调用者发送成功，并且将返回相关数据
