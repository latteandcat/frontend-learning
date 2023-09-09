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

- 我们在定义某些属性的时候，初衷其实是定义普通的属性，但是后面我们强行给它加上了数据属性描述符
- 如果我们想监听更加丰富的操作，比如新增属性、删除属性，那么 Object.defineProperty 是无能为力的

## Proxy 基本使用

在 ES6 中，新增了一个 Proxy 类，用于帮助我们创建一个代理

如果我们希望监听一个对象的相关操作，那么我们可以先创建一个代理对象（Proxy对象）

之后对该对象的所有操作，都通过代理对象来完成，代理对象可以监听我们想要对原对象进行哪些操作

创建 Proxy 对象：`const p = new Proxy(target, handler)`

```js
const obj = {
    name: "me",
    age: 19
}

// 创建 proxy 对象
const objProxy = new Proxy(obj, {
    // handler 中可以添加多个捕捉器
})

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
>早期的 ECMA 规范中没有考虑到这种对 **对象本身** 的操作如何设计会更加规范，所以将这些 API 放到了 Object 上面
>
>但是 Object 作为一个构造函数，这些操作实际上放到它身上并不合适
>
>另外还包含一些类似于 in、delete 操作符，让 JS 看起来是有一些奇怪
>
>所以在 ES6 中新增了 Reflect，让我们这些操作都集中到了 Reflect 对象上

在代理对象中使用 Reflect API 的好处

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

obj.name = "you"
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

3. 如果发送网络请求失败了，那么告知调用者发送失败，并且告知错误信息

```js
function requestData(url, successCallback, failureCallback) {
    setTimeout(() => {
        if (url === "http://hello.com") {
            successCallback("一组成功数据")
        } else {
            failureCallback("请求失败")
        }
    }, 1000)
}
```

ES5 之前处理异步任务都是这样通过回调函数封装异步处理代码，这种方案存在一些弊端

1. 需要自己来设计回调函数、回调函数的名称、回调函数的使用
2. 对于不同的人、不同的框架设计出来的方案是不同的，那么我们必须耐心去看别人的源码或者文档，以便可以理解它
   这个函数到底怎么用

## Promise 代码结构

Promise 是一个处理异步任务的类

- 在通过 new 创建 Promise 对象时，我们需要传入一个回调函数，我们称之为 executor
- executor 函数会被立即执行，并且传入另外两个回调函数 resolve 和 reject
- 当调用 resolve 回调函数时，会执行 Promise 对象的 then 方法传入的回调函数，Promise 状态变为 fullfilled
- 当调用 reject 回调函数时，会执行 Promise 对象的 catch 方法传入的回调函数，Promise 状态变为 rejected

```js
const promise = new Promise((resolve, reject) => {
    resolve("exec then callback")
    reject("exec catch callback")
})

promise.then(res => {
    console.log(res)
}).catch(err => {
    console.log(err)
})
```

Promise 在使用过程中分为三个状态

- 待定 pending： 初始状态，既没有被兑现，也没有被拒绝

  当执行 executor 中的代码时，处于该状态

- 已兑现 fulfilled：操作成功完成

  执行了 resolve 时，处于该状态，Promise 已经被兑现

- 已拒绝 rejected：操作失败

  执行了 reject 时，处于该状态，Promise 已经被拒绝

一旦状态被确定下来，Promise 的状态会被锁死，该 Promise 的状态是不可更改的

- 在调用 resolve 的时候，如果 resolve 传入的值本身不是一个 Promise，那么会将该 Promise 的状态变成 fulfilled
- 之后再次调用 resolve 或者 reject 已经不会有任何响应了，因为无法改变 Promise 状态

使用 Promise 重构异步处理代码

```js
function requestData(url) {
    retrun new Promise((resolve, reject) => {
        setTimeout(() => {
            if (url === "http://hello.com") {
                resolve("一组成功数据")
            } else {
                reject("请求失败")
            }
        }, 1000)
    })
}

requestData("http://hello.com").then(res => {
    console.log(res)
}).catch(err => {
    console.log(err)
})
```

## resolve 的传入值

1. 传入一个普通的值或者对象，这个值会作为 then 回调的参数

2. 传入另外一个 Promise，那么这个新的 Promise 会决定原 Promise 的状态

3. 传入一个有实现 then 方法的对象（thenable object），会执行该对象的 then 方法

   并且根据执行结果决定 Promise 的状态

   实现的 then 方法也将 resolve 和 reject 作为参数

## then 方法

then 方法是 Promise 对象上的一个方法（实例方法）

它其实是放在 Promise 的原型上的 Promise.prototype.then

**then 方法可以接收两个参数**

- fulfilled 的回调函数：当状态变成 fulfilled 时会回调的函数
- reject 的回调函数：当状态变成 reject 时会回调的函数

```js
promise.then(res => {
    console.log(res)
}, err => {
    console.log(err)
})
// 等价于
promise.then(res => {
    console.log(res)
}).catch(err => {
    console.log(err)
})
```

**then 方法可以被多次调用**

-  每次调用我们都可以传入对应的 fulfilled 回调
- 当 Promise 的状态变成 fulfilled 的时候，这些回调函数都会被执行

```js
promise.then(res => {
    console.log(res)
})

promise.then(res => {
    console.log(res)
})

promise.then(res => {
    console.log(res)
})
```

**then 方法的返回值**

then 方法本身是有返回值的，它的返回值是一个 Promise，所以我们可以在 then 后面进行链式调用

then 返回的 Promise 处于什么状态

- 当 then 方法中的回调函数处于执行状态的时候，返回的 Promise 处于 pending 状态
- 当 then 方法中的回调函数返回一个结果时
  - 返回一个普通的值，默认返回 undefined，那么它处于 fulfilled 状态，并且会将结果作为 resolve 的参数
  - 返回一个 Promise，新的 Promise 会决定原 Promise 的状态
  - 返回一个 thenable 对象，根据 then 方法的执行结果决定原 Promise 的状态

- 当 then 方法抛出一个异常时，那么它处于 rejected 状态

  `throw new Error("error message")`

## catch 方法

**catch 方法可以被多次调用**

catch 方法也是 Promise 对象上的一个方法（实例方法）

- 每次调用我们都可以传入对应的 reject 回调
- 当 Promise 的状态变成 rejected 的时候，这些回调函数都会被执行

**catch 方法的返回值**

catch 方法也会返回一个 Promise对象，所以我们可以在 catch 方法后面继续调用 then 方法或者 catch 方法

catch 返回的 Promise 的所处状态也是根据 catch 方法的返回值来决定的

**catch 方法的执行时机**

如果第一个 promise 被 reject 那么会执行最近的 catch

```js
promise.then(res => {
    
}).then(res => {
    
}).then(res => {
    
}).catch(err => {
    
})
```

如果 promise 被 reject 但是没有 catch 方法就会报错

## finally 方法

finally 是在ES9（ES2018）中新增的一个特性：表示无论 Promise 对象无论变成 fulfilled 还是 rejected 状态，最终都会被执行
的代码

```js
const promise = new Promise((resolve, reject) => {
    
})

promise.then(res => {
    
}).catch(err => {
    
}).finally(() => {
    
})
```

## resolve 方法

resolve 是 Promise 的类方法

Promise.resolve 的用法相当于 new Promise，并且执行 resolve 操作

```js
Promise.resolve("why")
// 等价于
new Promise((resolve) => resolve("why"))
```

resolve 参数的形态

1. 一个普通的值或对象
2. 一个 Promise
3. 一个 thenable 对象

## reject 方法

reject 类似于 resolve 方法，只是会将 Promise 对象的状态设置为 reject 状态

Promise.reject 的用法相当于 new Promise，并且执行 reject 操作

```js
Promise.reject("why")
// 等价于
new Promise((resolve, reject) => reject ("why"))
```

reject 传入的参数无论是什么形态，都会直接作为 rejected 状态的参数传递到 catch 的回调函数中

## all 方法

Promise.all 也是类方法的一个，作用是将多个 Promise 包裹在一起形成一个新的 Promise

新的 Promise 状态由包裹的所有 Promise 共同决定

- 当所有的 Promise 状态变成 fulfilled 状态时，新的 Promise 状态为 fulfilled，并且会将所有 Promise 的返回值组成一个数组
- 当有一个 Promise 状态为 reject 时，新的 Promise 状态为 reject，并且会将第一个 reject 的返回值作为参数

```js
const p1 = new Promise((resolve, reject) => {
    
})

const p2 = new Promise((resolve, reject) => {
    
})

const p3 = new Promise((resolve, reject) => {
    
})

Promise.all([p1, p2, p3]).then(res => {
    console.log(res)
}).catch(err => {
    console.log(err)
})
```

常用于一次性发送多个网络请求

## allSettled 方法

all 方法有一个缺陷：当有其中一个 Promise 变成 reject 状态时，新 Promise 就会立即变成对应的 reject 状态

对于 fulfilled 状态和 pending 状态的 Promise 获取不到对应的结果

ES11 中新增了新的 API：Promise.allSettled

- 该方法会在所有的 Promise 都有结果（settled），无论是 fulfilled，还是 rejected 时，才会有最终的状态

- 并且这个 Promise 的结果一定是 fulfilled

- allSettled 的结果是一个对象数组，数组中存放着每一个 Promise 的结果，每个结果对应一个对象

  这个对象中包含 status 状态以及对应的 value 值

```js
Promise.allSettled([p1, p2, p3]).then(res => {
    console.log(res)
}).catch(err => {
    console.log(err)
})
```

## race 方法

如果有一个 Promise 有了结果，我们就希望决定最终 Promise 的状态，那么可以使用 race 方法

```js
Promise.race([p1, p2, p3]).then(res => {
    console.log(res)
}).catch(err => {
    console.log(err)
})
```

race 会等到第一个结果决定最终状态，但是这个结果可能是 fulfilled 也可能是 rejected

## any 方法

any 方法是 ES12 新增的方法，和 race 方法类似

any 方法会等到一个 fulfilled 状态，才会决定新 Promise 的状态

如果所有的 Promise 都是 reject 的，那么也会等到所有的 Promise 都变成 rejected 状态

所有的 Promise 都是 reject 的情况下，会报一个 AggregateError 的错误

```js
Promise.any([p1, p2, p3]).then(res => {
    console.log(res)
}).catch(err => {
    console.log(err)
})
```

# Iterator

## 迭代器的定义

**迭代器的广义定义**

迭代器（iterator）是使用户在容器对象（container）上遍访的对象，使用该接口无需关心对象的内部实现细节

简单的讲，迭代器就是帮助我们对某个数据结构进行遍历的对象

**JS 中的迭代器定义**

JS 中的迭代器是一个符合**迭代器协议**的具体对象

迭代器协议的作用是定义产生一系列值的标准方式

JS 中的迭代器协议就是一个特定的 next 方法

- next 应是一个无参数或者有一个参数的函数

- next 应返回一个拥有两个属性的对象

  - done

    如果迭代器可以产生序列中的下一个值，则为 false（等价于没有指定 done）

    如果迭代器已将序列迭代完毕，则为 true （此时 value 是可选的，如果它依然存在，即为迭代结束之后的默认返回值）

  - value

    迭代器返回的任何 JS 值（具体值或者 undefined），done 为 true 时可省略

**迭代器的实现案例**

```js
const friends = ["a", "b", "c"]
let index = 0
const friendsIterator = {
    next: function() {
        if (index < friends.length) {
            return { done: false, value: friends[index++] }
        } else {
            return { done: true, value: undefined} // value 可省略
        }
    }
}

// 第二种写法
function createArrayIterator(arr) {
    let index = 0
    return {
        next: function() {
            if (index < arr.length) {
                return { done: false, value: arr[index++] }
            } else {
                return { done: true, value: undefined}
            }
        }
    }
}
const friendsIterator = createArrayIterator(friends)
```

## 可迭代对象

迭代器在单独使用的时候需要创建 index，再创建迭代器，比较麻烦

可迭代对象就是在对象内部实现对迭代器的封装

**可迭代对象的定义**

- 可迭代对象的要求是必须实现 `@@iterator` 方法，这个方法可以使用 `Symbol.iterator` 访问

- `@@iterator` 方法必须返回一个符合 iterable protocol 迭代器协议的迭代器

- 可迭代对象可以进行某些迭代操作，比如 `for of` 就会调用它的 `@@iterator` 方法

**可迭代对象的实现案例**

```js
const info = {
    friedns: ["a", "b", "c"],
    [Symbol.iterator]: function() { // @@iterator 方法
        let index = 0
        return { // 迭代器
            next: () => { // 使用箭头函数确保 this 指向 info
                if (index < this.friends.length) {
                    return { done: false, value: this.friends[index++] }
                } else {
                    return { done: true, value: undefined}
                }
            }
        }
    }
}
```

**原生迭代器对象**

事实上我们平时创建的很多原生对象已经实现了可迭代协议，会生成一个迭代器对象

- String
- Map
- Array
- Set
- arguments 对象
- NodeList 集合

```js
const names = ["a", "b", "c"]
const iterator = names[Symbol.iterator]()

console.log(iterator.next())
console.log(iterator.next())
console.log(iterator.next())
console.log(iterator.next())
```

## 可迭代对象的应用

1. 用于 JS 特定语法
   - for of
   - 展开语法
   - yield*
   - 解构赋值
2. 创建对象
   - new Map([Iterable])
   - new WeakMap([Iterable])
   - new Set([Iterable])
   - new WeakSet([Iterable])
3. 调用方法
   - Promise.all(iterable)
   - Promise.race(iterable)
   - Array.from(iterable)

## 自定义可迭代类

在类的定义中添加上 `@@iterator` 公共实例方法，就可以让该类的实例对象都变成可迭代对象

实现代码

```js
class ClassRoom {
    constructor(roomId, roomAddress, students) {
        this.roomId = roomId
        this.roomAddress = roomAddress
        this.students = students || []
    }
    
    enterClassRoom(studentName) {
        this.students.push(studentName)
    }
    
    [Symbol.iterator]() {
        let index = 0
        return {
            next: () => {
                if (index < this.students.length) {
                    return { done: false, value: this.students[index++] }
                } else {
                    return { done: true }
                }
            }
        }
    }
}

const classRoom = new ClassRoom("301", "9号楼", ["A", "B", "C"])
for (const s of classRoom) {
    console.log(s)
}
```

## 迭代器的中断

迭代器在某些情况下会在没有完全迭代的情况下中断

- 比如遍历的过程中通过break、return、throw中断了循环操作
- 比如在解构的时候，没有解构所有的值

在迭代器中添加 return 方法可以监听迭代的中断

```js
[Symbol.iterator]() {
    let index = 0
    return {
        next: () => {
            if (index < this.students.length) {
                return { done: false, value: this.students[index++] }
            } else {
                return { done: true }
            }
        },
        return: () => {
            console.log("迭代器提前终止了")
            return { done: true }
        } 
    }
}

for (const s of classRoom) {
    console.log(s)
    if (s === "C") {
        break
    }
}
```

# Generator

## 生成器的定义

生成器是 ES6 中新增的一种函数控制、使用的方案

它可以让我们更加灵活地控制函数什么时候继续执行、暂停执行等

生成器函数和普通函数的区别

1. 生成器函数需要在function的后面加一个符号：*

2. 生成器函数可以通过 yield 关键字来控制函数的执行流程

3. 生成器函数的返回值是一个 Generator

   生成器事实上是一种特殊的迭代器

   MDN：Instead, they return a special type of iterator, called a Generator

## 生成器函数的执行

通过 next() 可以执行生成器函数中的代码，但遇到 yield 会中断执行

next 的默认返回值是 undefined，可以通过 yield 来返回一段代码的结果

```js
function* foo() {
    console.log("函数开始执行")
    const value1 = 100
    console.log(value1)
    yield value1
    
    const value2 = 200
    console.log(value2)
    yield value2
    
    const value3 = 300
    console.log(value3)
    yield value3
    
    console.log("函数结束执行")
}

const fooGenerator = foo()

// 执行到第一个 yield 并且暂停
console.log(fooGenerator.next())

// 执行到第二个 yield 并且暂停
console.log(fooGenerator.next())

// 执行到第三个 yield 并且暂停
console.log(fooGenerator.next())

// 执行剩余的代码
console.log(fooGenerator.next())

```

## 生成器传递参数

在调用 next 方法的时候，可以给它传递参数

那么这个参数会作为一个上一个 yield 语句的返回值

```js
function* foo(init) {
    console.log("函数开始执行");

    const value1 = yield init + "aaa"
    console.log(value1); // helloaaa

    const value2 = yield value1 + "bbb"
    console.log(value2); // helloaaabbb

    const value3 = yield value2 + "ccc"
    console.log(value3); // undefined

    console.log("函数结束执行");
}

const generator = foo("hello")
const res1 = generator.next()
console.log(res1); // {value: 'helloaaa', done: false}
const res2 = generator.next(res1.value)
console.log(res2); // {value: 'helloaaabbb', done: false}
const res3 = generator.next(res2.value)
console.log(res3); // {value: 'helloaaabbbccc', done: false}
const res4 = generator.next()
console.log(res4); // {value: undefined, done: true}
```

## 生成器的提前结束

还有一个可以给生成器函数传递参数的方法是通过 return 函数

`return()` 方法返回给定的值并结束生成器

```js
function* bar() {
    const value1 = yield "why"
    console.log("value1:", valuel)
    const value2 = yield value1 
    const value3 = yield value2
}

const barGenerator = bar()
console.log(barGenerator.next()) // { value: 'why', done: false }
console.log(barGenerator.return(123)) // { value: 123, done: true }
console.log(barGenerator.next()) // { value: undefined, done: true }
```

## 生成器抛出异常

除了给生成器函数内部传递参数之外，也可以给生成器函数内部抛出异常

`throw()` 方法用来向生成器抛出异常，并恢复生成器的执行

抛出异常后我们可以在生成器函数中通过 `try...catch` 块捕获异常

```js
function* foo() {
    try {
        yield "why"
    } catch (error) {
        console.log("内部捕获异常：", error);
    }
    yield "why"
}

const generator = foo()
console.log(generator.next()) // { value: 'why', done: false }
generator.throw(new Error("something went wrong"))
console.log(generator.next()) // { value: undefined, done: true }
```

## 生成器的应用

生成器可以在某些情况下可以替代迭代器

```js
const friends = ["a", "b", "c"]

function createArrayIterator(arr) {
    let index = 0
    return {
        next: function() {
            if (index < arr.length) {
                return { done: false, value: arr[index++] }
            } else {
                return { done: true, value: undefined}
            }
        }
    }
}

const friendsIterator = createArrayIterator(friends)

// 生成器的写法
function* createArrayIterator(arr) {
    for (let i = 0; i < arr.length; i++) {
        yield arr[i]
    }
}

// 语法糖的写法
function* createArrayIterator(arr) {
    yield* arr
}
```

`yield*` 可以依次迭代一个可迭代对象，每次迭代其中的一个值

自定义可迭代类的代码优化

```js
class ClassRoom {
    constructor(roomId, roomAddress, students) {
        this.roomId = roomId
        this.roomAddress = roomAddress
        this.students = students || []
    }
    
    enterClassRoom(studentName) {
        this.students.push(studentName)
    }
    
    *[Symbol.iterator]() {
        yield* this.students
        // yield* Object.entries(this)
    }
}
```

生成器是一种特殊的迭代器，也是可迭代对象，所以生成器也可以在可迭代对象的应用场景中使用

```js
function* createArrayIterator(arr) {
    yield* arr
}
const friends = ["a", "b", "c"];
const iter = createArrayIterator(friends);

for (const item of iter) {
    console.log(item);
}
```

## 生成器优化异步处理

多次递进异步请求的 Promise 实现方案

```js
function request(url) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve(url);
        }, 1000);
    });
}

function getData(url) {
    request(url).then(res1 => {
        console.log(res1);
        return request(res1 + "aaa")
    }).then(res2 => {
        console.log(res2);
        return request(res2 + "bbb")
    }).then(res3 => {
        console.log(res3);
        return request(res3 + "ccc")
    }).then(res4 => {
        console.log(res4);
    })
}

getData("hello")
```

生成器方案

```js
function* getData(url) {
    const res1 = yield request(url);
    console.log(res1);
    const res2 = yield request(res1 + "aaa");
    console.log(res2);
    const res3 = yield request(res2 + "bbb");
    console.log(res3);
    const res4 = yield request(res3 + "ccc");
    console.log(res4);
}

const gen = getData("hello");

// 手动执行 generator
gen.next().value.then(res1 => {
    gen.next(res1).value.then(res2 => {
        gen.next(res2).value.then(res3 => {
            gen.next(res3).value.then(res4 => {
                gen.next(res4)
            });
        });
    });
});

// 自动执行 generator 函数，通过递归实现，可扩展
function autoExecGenerator(generator) {
    function exec(res) {
        const result = generator.next(res);
        if (result.done) return result.value;
        result.value.then(res => {
            exec(res);
        });
    }
    exec();
}
// autoExecGenerator(gen)
```

async/await 方案

```js
async function getData(url) {
    const res1 = await request(url);
    console.log(res1);
    const res2 = await request(res1 + "aaa");
    console.log(res2);
    const res3 = await request(res2 + "bbb");
    console.log(res3);
    const res4 = await request(res3 + "ccc");
    console.log(res4);
}

getData("hello")
```

# async/await



