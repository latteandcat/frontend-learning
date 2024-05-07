# Vue 基础概念

Vue 是一套用于构建用户界面的渐进式 JavaScript 框架

## 使用方式

1. 通过 CDN 引入

   `<script src="https://unpkg.com/vue@next"></script>`

2. 下载 js 源码，手动引入

   `<script src="../js/vue.js"></script>`

3. 通过 npm 安装

4. 通过脚手架创建项目

## 声明式编程

命令式编程和声明式编程

- 命令式编程关注 “how to do”，由开发者完成 “how” 的过程

  每完成一个操作，都需要通过 JavaScript 编写一条代码，来给浏览器一个指令

  早期的原生 JS 和 Jquery 都使用命令式编程

- 声明式编程关注 “what & when to do”，由框架完成 “how” 的过程

  声明需要的内容，模板 template、数据 data、方法 methods 等

  声明以后只需要告诉框架什么时间做什么事情，具体的操作由框架完成
  
  Vue、React、Angular 和小程序使用声明式编程

## 创建项目

### Vue CLI

Vue CLI 是用于搭建 Vue 项目的脚手架工具

- CLI：Command-Line Interface
- 我们可以通过 CLI 选择项目的配置并创建项目
- Vue CLI 已经内置了 webpack 相关的配置

Vue CLI 的使用

- 安装：`npm install @vue/cli -g`

- 升级：`npm update @vue/cli -g `

- 创建项目：`vue create 项目名称`

[vue-cli 安装时切换包管理器](https://blog.csdn.net/weixin_41597344/article/details/122971639)

### Vite

[Vite](https://cn.vitejs.dev/) 是一个轻量级的、速度极快的构建工具

使用 Vite 创建 Vue 项目：`npm create vue@latest`

这个命令会安装和执行 create-vue，它是 Vue 提供的官方脚手架工具

构建一个 Vite + Vue 项目

```cmd
# npm 7+, extra double-dash is needed:
npm create vite@latest my-vue-app -- --template vue

# yarn
yarn create vite my-vue-app --template vue

# pnpm
pnpm create vite my-vue-app --template vue

# bun
bunx create-vite my-vue-app --template vue
```

# Vue 模板语法

## 插值

最基本的数据绑定形式是文本插值，它使用的是“Mustache”语法 (即双大括号)

1. 插入响应式属性，属性更改时同步更新

   `{{ msg }}`

2. 插入 JS 表达式

   ```js
   {{ number + 1 }}
   
   {{ ok ? 'YES' : 'NO' }}
   
   {{ message.split('').reverse().join('') }}
   ```

3. 调用 methods 中的函数

   ```js
   {{ reverse(msg) }}
   ```

注意

- 双大括号会将数据解释为纯文本，而不是 HTML

- 双大括号不能在 HTML attributes 中使用

- 插入表达式时仅支持单一表达式

  ```js
  <!-- 这是一个语句，而非表达式 -->
  {{ var a = 1 }}
  
  <!-- 条件控制也不支持，请使用三元表达式 -->
  {{ if (ok) { return message } }}
  ```

## 指令

- 指令是带有 `v-` 前缀的特殊 attribute

- 指令 attribute 的期望值为一个单一 JavaScript 表达式（少数特殊指令除外）

- 指令的任务是在其表达式的值变化时响应式地更新 DOM

- 完整的指令语法

  ![](../images/directive.png)

### v-bind

用于动态绑定属性

可以绑定一个或者多个属性值，或者向另一个组件传递 props 值

**基础用法**

```html
<template id="my-app">
  <!-- 完整写法 -->
  <img v-bind:src="src" alt="">
  <!-- 语法糖写法 -->
  <img :src="src" alt="">
  <!-- 动态属性名 -->
  <button v-bind:[key]="value"></button>
  <!-- 动态属性名的语法糖写法 -->
  <button :[key]="value"></button>、
  <!-- 内联字符串拼接 -->
	<img :src="'/path/to/images/' + fileName" />
</template>
```

**绑定类**

```html
<!-- 绑定类名 -->
<div :class="className"></div>

<!-- 绑定对象 -->
<div :class="{ active: isActive }"></div>
<!-- 动态类会和普通类共存 -->
<div class="static" :class="{ active: isActive, 'text-danger': hasError }"></div>
<!-- 可以直接绑定data中的对象、methods返回的对象或者计算属性返回的对象 -->
<div :class="classObject"></div>

<!-- 绑定数组 -->
<div :class="[activeClass, errorClass]"></div>
<!-- 数组中可以嵌入三元表达式 -->
<div :class="[isActive ? activeClass : '', errorClass]"></div>
<!-- 数组中也可以直接嵌入对象 -->
<div :class="[{ active: isActive }, errorClass]"></div>
```

**绑定样式**

```html
<!-- 绑定对象 -->
<div :style="{ color: activeColor, fontSize: fontSize + 'px', 'background-color': 'blue' }"></div>
<div :style="{ 'font-size': fontSize + 'px' }"></div>
<div :style="styleObject"></div>

<!-- 绑定数组 -->
<div :style="[baseStyles, overridingStyles]"></div>
```

**直接绑定对象**

```js
<!-- 可以将一个对象的所有属性绑定到元素上 -->
<div v-bind="info"></div>
```

### v-on

用于给元素绑定事件监听器

**基础用法**

```html
<!-- 方法处理函数 -->
<button v-on:click="doThis"></button>

<!-- 动态事件 -->
<button v-on:[event]="doThis"></button>

<!-- 内联声明 -->
<button v-on:click="doThat('hello', $event)"></button>

<!-- 语法糖写法 -->
<button @click="doThis"></button>

<!-- 动态事件的语法糖写法 -->
<button @[event]="doThis"></button>

<!-- 对象语法可以绑定多个事件（不支持修饰符） -->
<button v-on="{ mousedown: doThis, mouseup: doThat }"></button>
```

**添加修饰符**

修饰符相当于对事件进行了一些特殊的处理

- `.stop`：调用 event.stopPropagation() 阻止事件传递

- `.prevent`：调用 event.preventDefault() 取消事件的默认行为

- `.capture`：添加事件侦听器时使用 capture 模式（捕获模式）

- `.self`：只有当事件是从侦听器绑定的元素本身触发时才触发回调

  `event.target === event.currentTarget`

- `.{keyAlias}`：只在某些按键下触发处理函数

- `.once`：最多触发一次处理函数

- `.left`：只在鼠标左键事件触发处理函数

- `.right`：只在鼠标右键事件触发处理函数

- `.middle`：只在鼠标中键事件触发处理函数

- `.passive`：通过 `{ passive: true }` 附加一个 DOM 事件

```html
<!-- 停止传播 -->
<button @click.stop="doThis"></button>

<!-- 阻止默认事件 -->
<button @click.prevent="doThis"></button>

<!-- 不带表达式地阻止默认事件 -->
<form @submit.prevent></form>

<!-- 链式调用修饰符，调用顺序会对修饰符作用有影响 -->
<button @click.stop.prevent="doThis"></button>

<!-- 调用顺序会对修饰符作用有影响 -->
<!-- @click.prevent.self 会阻止元素及其子元素的所有点击事件的默认行为 -->
<!-- @click.self.prevent 则只会阻止对元素本身的点击事件的默认行为 -->

<!-- 按键用于 keyAlias 修饰符-->
<input @keyup.enter="onEnter" />

<!-- 点击事件将最多触发一次 -->
<button v-on:click.once="doThis"></button>

<!-- 滚动事件的默认行为 (scrolling) 将立即发生而非等待 `onScroll` 完成 -->
<!-- 以防其中包含 `event.preventDefault()` -->
<div @scroll.passive="onScroll">...</div>
```

**传递参数**

```html
<!-- 默认会传入 event 对象 -->
<button v-on:click="doThis"></button>

<!-- 需要传入其他参数时，可以通过 $event 传入 event 对象 -->
<button v-on:click="doThat('hello', $event)"></button>
```

### v-for

基于原始数据多次渲染元素或模板块

**遍历数组**

```html
<div v-for="item in items"></div>
<div v-for="(item, index) in items"></div>
```

Vue 能够侦听响应式数组的变更方法，并在它们被调用时触发相关的更新

主要包含一些不会修改原数组的方法

会修改原数组的方法需要对旧数组进行替换

**遍历对象**

遍历的顺序会基于对该对象调用 `Object.keys()` 的返回值来决定

```html
<div v-for="value in object"></div>
<div v-for="(value, key) in object"></div>
<div v-for="(value, key, index) in object"></div>
```

**其他用法**

v-for 也可以遍历数字、字符串及其他可迭代对象

```html
<!-- n 从 1 开始 -->
<span v-for="n in 10">{{ n }}</span>
```

**注意事项**

`v-for` 和 `v-if` 共存在一个节点上时 `v-if` 的优先级更高

所以 `v-if` 的条件将无法访问到 `v-for` 作用域内定义的变量别名

推荐在外面包装一层 template 元素可以解决这个问题

```html
<template v-for="todo in todos">
  <li v-if="!todo.isComplete">
    {{ todo.name }}
  </li>
</template>
```

**数组更新检测**

Vue 将被侦听的数组的变更方法进行了包裹

所以它们也将会触发视图更新

比如：`push / pop / shift / unshift / splice / sort / reverse`

这些方法会修改原来的数组

但是某些方法不会替换原来的数组，而是会生成新的数组

比如：`filter / concat / slice`

> **v-for 中 key 的作用**
>
> Vue 默认按照“就地更新”的策略来更新通过 v-for 渲染的元素列表。
>
> 当数据项的顺序改变时，Vue 不会随之移动 DOM 元素的顺序，
>
> 而是就地更新每个元素，确保它们在原本指定的索引位置上渲染。
>
> key 可以帮助 Vue 跟踪每个节点的标识，从而重用和重新排序现有的元素。
>
> [Vue 虚拟 DOM](https://juejin.cn/post/6997579802215448606)
>
> - 虚拟节点 VNode 本质上是一个节点描述对象，用于创建真实的 DOM 节点
>
>   template -> 虚拟节点 VNode -> 虚拟 DOM Tree -> 真实 DOM Tree
>
> - 虚拟 DOM 是 VNode 组成的树，是对真实 DOM 的抽象，用于比较虚拟节点和旧虚拟节点并更新视图
>   - 优势 1：具备跨平台的优势
>   - 优势 2：减少 DOM 操作，提高运行效率
>   - 优势 3：合理高效地更新视图，提升渲染性能

### v-model

v-model 用于在表单 `input / textarea / select` 等元素上创建双向数据绑定

它会根据控件类型自动选取正确的方法来更新元素

v-model 本质上是一种语法糖，负责监听用户的输入事件来更新数据

```html
<input v-model="searchText">
<!-- 等价于 -->
<input :value="searchText" @input="searchText = $event.target.value">
```

其他使用案例

1. textarea 的绑定值也是字符串

2. checkbox 为单选框时绑定值为布尔值

   为多选框时绑定值为数组，数组中包含选中项的 value 属性

3. radio 的绑定值为字符串，对应选中项的 value 属性

4. select 为单选时绑定值时字符串，对应选中 option 的 value 属性

   为多选框时绑定的值为数组，数组中包含选中 option 的 value 属性

v-model 的修饰符

- `.lazy`

  默认情况下，v-model 在进行双向绑定时，绑定的是 input 事件

  lazy 修饰符会将绑定的事件切换为 change 事件

- `.number`

  会将绑定值转化为数字类型

- `.trim`

  去除绑定值的首尾空格

### 条件渲染指令

- `v-if`

- `v-else-if`

- `v-else`

- `v-show`

**v-show 和 v-if 的区别**

1. 用法不同

   - v-show 不支持在 template 元素上使用

   - v-show 不可以和 v-else 一起使用

2. 本质不同

   - v-if 是按条件渲染，在切换时会对渲染的内容进行销毁和重建

     v-if 是惰性渲染，如果条件为 false 就不会渲染或者会被销毁掉，当条件为 true 时才会渲染

   - v-show 无论初始条件如何，始终会被渲染，只是通过 CSS 的 display 属性来切换显示状态

3. 应用场景不同

   - v-if 有更高的切换开销，适合绑定条件很少改变的情况
   - v-show 有更高的初始渲染开销，适合需要频繁切换条件的情况

### 其他指令

- `v-once` ：用于指定元素或者组件只渲染一次

  当数据发生变化时，元素或者组件以及其所有的子元素将视为静态内容并且跳过

  该指令可以用于性能优化

- `v-text`：用于更新元素的 textContent

- `v-html`：用于更新元素的 innerHTML

- `v-pre`：用于跳过元素和它的子元素的编译过程，显示原始的 Mustache 标签

  跳过不需要编译的节点可以加快编译的速度

- `v-cloak`：这个指令会保持在元素上知道关联组件实例结束编译

  和 `[v-cloak] { display: none; }` 一起使用

  可以隐藏未编译的 Mustache 标签直到组件实例准备完毕

- `v-memo`：用于缓存模板

  传入一个固定长度的依赖值数组进行比较，如果数组里的每个值都与最后一次的渲染相同，那么整个子树的更新将被跳过

  传入空数组与 `v-once` 效果相同
  
  仅用于性能至上场景中的微小优化

# Vue Options API

## data

```js
Vue.createApp({
  data() {
    return {
      counter: 0
    }
  }
}).mount('#app')
```

data 属性需要传入一个函数，且该函数会返回一个对象

这个返回的对象会被 Vue 的响应式系统劫持，之后对该对象的修改或者访问都会在劫持中被处理

## methods

```js
Vue.createApp({
  data() {
    return {
      counter: 0
    }
  },
  methods: {
    increment() {
      this.counter++
    },
    decrement() {
      this.counter--
    }
  }
}).mount('#app')
```

methods 是一个用于定义方法的对象

这些方法中可以使用 this 关键字访问 data 返回的对象中的属性

但 methods 中的方法不能使用箭头函数

1. 为什么不能使用箭头函数

   使用箭头函数时，方法中的 this 会去上层作用域查找 this

   最终找到的就是 script 中的 this 就是 window

2. 不使用箭头函数的情况下，this 到底指向的是什么

   方法中的 this 绑定的是实例的代理也就是 data 返回的对象

>**复杂数据的处理方式**
>
>插值语法可以直接显示数据，但是有时候需要对数据进行一些处理后显示
>
>1. 直接在模板中实现处理逻辑
>  - 缺点 1：模板中存在太多复杂逻辑，不便于维护
>  - 缺点 2：可能会在多个地方写相同的处理逻辑，不利于降低代码重复性
>  - 缺点 3：多次使用的时候，运算会多次执行，没有缓存
>
>2. 通过 methods 中的方法返回处理后的内容
>  - 缺点 1：数据的处理变成了方法的调用
>  - 缺点 2：多次使用的时候，方法也会多次执行，没有缓存
>3. 使用计算属性 computed
>  - 优点 1：虽然是一个函数但是可以当成属性使用
>  - 优点 2：计算属性是有缓存的，不会重复执行多次

## computed

计算属性用于编写**任何包含响应式数据的复杂逻辑**

计算属性将被混入到组件实例中

所有 getter 和 setter 的 this 上下文自动地绑定为组件实例

```js
computed: {
  fullname() {
    return this.firstName + this.lastName
  },
  scoreLevel() {
    this.score >= 60 ? "及格" : "不及格"
  },
  reverseMessage() {
    return this.message.split(" ").reverse().join(" ")
  }
}
```

计算属性的缓存

- 计算属性会基于它们的依赖关系进行缓存
- 在数据不发生变化时，计算属性是不需要重复计算的
- 但是如果依赖的数据发生变化，在使用时，计算属性依然会进行重复计算

计算属性的 setter 和 getter

- 计算属性在大多数情况下，只需要一个 getter 方法即可

  所以我们会将计算属性直接写成一个函数

- 计算属性的完整写法是设置 set 和 get 方法

```js
computed: {
  fullname() {
    get() {
      return this.firstName + this.lastName
    },
    set(value) {
      const names = value.split(" ")
      this.firstName = names[0]
      this.lastName = names[1]
    }
  }
}
```

## watch

侦听器 watch 用于在代码逻辑中**监听某个数据的变化**

```js
Vue.createApp({
  data() {
    return {
      counter: 0
    }
  },
  watch: {
    counter(newValue, oldValue) {
      console.log(newValue, oldValue)
      // 如果是对象类型，拿到的是相应的代理对象
      // 可以通过 { ...newValue } 或者 Vue.toRaw(newValue) 获取原生对象
    }
  }
}).mount('#app')
```

侦听器的配置选项

- `deep`：开启深层侦听

  watch 默认是浅层的，只有在被侦听的属性被赋新值时，才会触发回调函数

  嵌套属性的变化不会触发，深层侦听可以侦听所有嵌套的变更

- `immediate`：开启即时回调

  watch 默认是懒执行的，仅当数据源变化时，才会执行回调

  即时回调会在创建侦听器时，立即执行一遍回调

  比如：请求一些初始数据，然后在相关状态更改时重新请求数据

```js
watch: {
  counter: {
    handler(newValue, oldValue) {
      console.log(newValue, oldValue)
    },
    deep: true,
  	immediate: true
  }
}
```

监听嵌套属性也可以使用 `.` 的方式

```js
export default {
  watch: {
    // 注意：只能是简单的路径，不支持表达式。
    'some.nested.key'(newValue, oldValue) {
      // ...
    }
  }
}
```

侦听器的其他使用方式

```js
export default {
  watch: {
    // 将 method 中的方法作为回调
    b: "someMethod",
    // 传入一个回调数组，会被逐一调用
    f: [
      "handler1",
      function handler2(newValue, oldValue) {
        // ...
      },
      {
        handler: function handler3(newValue, oldValue) {
          // ...
        }
      }
    ]
  }
}
```

使用组件实例的 `$watch()` 方法可以命令式地创建一个侦听器

- 第一个参数是要侦听的源
- 第二个参数是侦听的回调函数 callback
- 第三个参数是侦听的配置选项
- 返回值是 `unwatch()` 函数，可以用于停止侦听

```js
created() {
  this.$watch('message', (newValue, oldValue) => {
    console.log(newValue, oldValue)
  }, {deep: true, immediate: true})
}
```
