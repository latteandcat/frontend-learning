# Vue 基础概念

## 使用方式

1. 通过 CDN 引入
2. 下载源码，手动引入
3. 通过 npm 安装
4. 通过脚手架创建项目

## 声明式编程

每完成一个操作，都需要通过 JavaScript 编写一条代码，来给浏览器一个指令

这样的编写代码的过程就叫做命令式编程

声明式编程就是声明需要的内容，模板 template、数据 data、方法 methods 等

声明以后只需要告诉框架什么时间做什么事情，具体的操作由框架完成

换句话说

- 命令式编程关注 “how to do”
- 声明式编程关注 “what & when to do”，由框架完成 “how” 的过程

## data

```js
Vue.createApp({
  data() {
    counter: 0
  }
}).mount('#app')
```

data 属性需要传入一个函数，且该函数会返回一个对象

这个返回的对象会被 Vue 的响应式系统劫持，之后对该对象的修改或者访问都会在劫持中被处理

## methods

```js
Vue.createApp({
  data() {
    counter: 0
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

这些方法中可以使用 this 访问 data 返回的对象中的属性

但 methods 中的方法不能使用箭头函数，否则 this 的指向就会改变

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
  <!-- 动态属性名的缩写形式 -->
  <button :[key]="value"></button>、
  <!-- 内联字符串拼接 -->
	<img :src="'/path/to/images/' + fileName" />
</template>
```

**绑定类**

```html
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
<div :style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
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

给元素绑定事件监听器

**基础用法**

```html
<!-- 方法处理函数 -->
<button v-on:click="doThis"></button>

<!-- 动态事件 -->
<button v-on:[event]="doThis"></button>

<!-- 内联声明 -->
<button v-on:click="doThat('hello', $event)"></button>

<!-- 缩写 -->
<button @click="doThis"></button>

<!-- 使用缩写的动态事件 -->
<button @[event]="doThis"></button>

<!-- 对象语法（不支持修饰符） -->
<button v-on="{ mousedown: doThis, mouseup: doThat }"></button>
```

**添加修饰符**

```html
<!-- 停止传播 -->
<button @click.stop="doThis"></button>

<!-- 阻止默认事件 -->
<button @click.prevent="doThis"></button>

<!-- 不带表达式地阻止默认事件 -->
<form @submit.prevent></form>

<!-- 链式调用修饰符 -->
<button @click.stop.prevent="doThis"></button>

<!-- 按键用于 keyAlias 修饰符-->
<input @keyup.enter="onEnter" />

<!-- 点击事件将最多触发一次 -->
<button v-on:click.once="doThis"></button>
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

遍历的顺序会基于对该对象调用 Object.keys() 的返回值来决定

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

v-for 和 v-if 共存在一个节点上时 v-if 的优先级更高

所以 `v-if` 的条件将无法访问到 `v-for` 作用域内定义的变量别名

推荐在外面包装一层 template 元素可以解决这个问题

```html
<template v-for="todo in todos">
  <li v-if="!todo.isComplete">
    {{ todo.name }}
  </li>
</template>
```

> **v-for 中 key 的作用**
>
> Vue 默认按照“就地更新”的策略来更新通过 v-for 渲染的元素列表。当数据项的顺序改变时，Vue 不会随之移动 DOM 元素的顺序，而是就地更新每个元素，确保它们在原本指定的索引位置上渲染。
>
> key 可以帮助 Vue 跟踪每个节点的标识，从而重用和重新排序现有的元素。

### 条件渲染指令

- `v-if`

- `v-else-if`

- `v-else`

- `v-show`

> v-show 和 v-if 的区别
>
> 1. 用法不同
>
>    - v-show 不支持在 template 元素上使用
>
>    - v-show 不可以和 v-else 一起使用
>
> 2. 本质不同
>
>    - v-if 是按条件渲染，在切换时会对渲染的内容进行销毁和重建
>
>      v-if 也是惰性渲染，如果初始条件为 false 就不会渲染，只有条件首次变为 true 才会渲染
>
>    - v-show 无论初始条件如何，始终会被渲染，只是通过 CSS 的 display 属性来切换显示状态
>
> 3. 应用场景不同
>
>    - v-if 有更高的切换开销，适合绑定条件很少改变的情况
>    - v-show 有更高的初始渲染开销，适合需要频繁切换条件的情况

### 其他指令

- `v-once` ：用于指定元素或者组件只渲染一次

  当数据发生变化时，元素或者组件以及其所有的子元素将视为静态内容并且跳过

  该指令可以用于性能优化

- `v-text`：用于更新元素的 textContent

- `v-html`：用于更新元素的 innerHTML

- `v-pre`：用于跳过元素和它的子元素的编译过程，显示原始的 Mustache 标签

  跳过不需要编译的节点，加快编译的速度

- `v-cloak`：用于隐藏尚未完成编译的 DOM 模板

  和 `[v-cloak] { display: none; }` 一起使用可以隐藏未编译的 Mustache 标签直到组件实例准备完毕

- `v-memo`：用于缓存模板

  传入一个固定长度的依赖值数组，如果数组里的每个值都与最后一次的渲染相同，那么整个子树的更新将被跳过

  仅用于性能至上场景中的微小优化
