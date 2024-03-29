# 认识 DOM 和 BOM

![](../images/window.png)

window 对象中除了 JS 的一些内置类和对象以外

还包含一些浏览器提供的 DOM、BOM 相关的 API，用于对页面和浏览器进行操作

- DOM：文档对象模型（Document Object Model）

  将页面所有的内容表示为可以修改的对象

- BOM：浏览器对象模型（Browser Object Model）

  由浏览器提供的用于处理文档之外的所有内容的其他对象

  比如：navigator、location、history 等对象

通俗的讲 

- DOM 就是 JS 和 HTML 文档之间的桥梁
- BOM 就是 JS 和浏览器之间的桥梁

# DOM

## 理解 DOM

**DOM 文档对象模型**

- 浏览器会将我们编写在 HTML 中的每一个元素抽象成一个个对象
- 这些对象都可以通过 JS 进行访问和修改
- 这个将元素抽象成对象的过程就叫做 DOM

**document 对象**

Document 节点表示整个载入的网页

它的实例是全局的 document 对象

- 整个 HTML 文档都会被抽象到 document 对象中
- 对 DOM 的所有操作都是从 document 对象开始的
- document 对象是 DOM 的入口点，可以从 document 开始去访问任何节点元素 

对于最顶层的 html、head、body 元素，我们可以直接在 document 对象中获取到

- document.documentElement -> html 元素
- document.body -> body 元素
- document.head -> head 元素
- document.doctype -> 文档声明

**DOM Tree**

- 页面中所有的元素最终会形成一个树结构

- 在将页面中的元素抽象成 DOM 对象时

  这些元素对应的对象也会形成一个树结构，称之为 DOM Tree

**DOM 的继承关系**

![](../images/dom-extends.png)

## navigator 导航

### 节点导航

如果我们获取到一个节点（Node）后，可以根据这个节点去获取其他的节点，我们称之为节点之间的导航

节点之间存在如下的关系：

- 父节点：parentNode
- 前兄弟节点：previousSibling
- 后兄弟节点：nextSibling
- 子节点：childNodes
- 第一个子节点：firstChild
- 最后一个子节点：lastChild

![](../images/node-navigator.png)

### 元素导航

如果我们获取到一个元素（Element）后，可以根据这个元素去获取其他的元素，我们称之为元素之间的导航

元素之间存在如下的关系

- 父元素：parentElement
- 前兄弟元素：previousElementSibling
- 后兄弟元素：nextElementSibling
- 子元素：children
- 第一个子元素：firstElementChild
- 最后一个子元素：lastElementChild

![](../images/element-navigator.png)

### 表格导航

`<table>` 元素支持以下属性获取其他元素

- `table.rows`：`<tr>` 元素的集合
- `table.caption`：表格中的 `<caption>` 元素
- `table.tHead`：表格中的 `<thead>` 元素
- `table.tFoot`：表格中的 `<tfoot>` 元素
- `table.tBodies`：`<tbody>` 元素的集合

`<thead>` `<tfoot>` `<tdody>` 元素都提供了 `rows` 属性获取其内部 `<tr>` 元素的集合

`<tr>` 的属性

- `tr.cells`：`<tr>` 中 `<td>` 元素和 `<th>` 元素的集合
- `tr.sectionRowIndex`：`<tr>` 在`<thead>` `<tfoot>` `<tdody>` 中的索引
- `tr.rowIndex`：`<tr>` 在整个表格中的索引

`<td>` 和 `<th>` 的属性

- `td.cellIndex`：在 `<tr>` 中单元格的编号

### 表单导航

- `doucment.forms`：获取所有表单的集合

- `form.elements`：获取 form 的所有子元素

  设置表单子元素的 name 属性之后可以直接通过 `form.elements.name` 获取对应的子元素

  然后通过 value 属性就可以获得对应的值
  
  ```js
  var formEl = document.forms[0]
  var elements = formEl.elements
  console.log(elements.username.value)
  ```

## 获取元素

导航属性在元素彼此靠近或者相邻时可以通过元素之间的关系获取元素

但导航属性并不能任意地获取到某一个元素

DOM 提供了获取元素和元素集合的一系列方法

<img src="../images/getElement.png" style="zoom: 67%;" />

目前开发中最常用的是 `querySelector` 和 `querySelectorAll`

`getElementById` 偶尔也会使用或者用于适配低版本浏览器

`querySelectorAll` 或者其他方法获取到的对象节点也是可迭代对象，可以用 `for of` 遍历

## 节点属性

- `nodeType`：获取节点的类型

  - nodeType 属性可用于区分不同类型的节点，比如 元素, 文本 和 注释。
  - nodeType 是一个代表节点类型的整数

  <img src="../images/node-type.png" style="zoom: 67%;" />

- `nodeName`：获取节点的名字

  nodeName 适用于任意节点

- `tagName`：获取元素的标签名称

  tagName 仅适用于 Element 节点

  元素节点的 nodeName 和 tagName 相同

- `innerHTML`
  
  - 将元素中的 HTML 获取为字符串形式
  - 设置元素中的内容
- `outerHTML`
  
  - 包含了元素的完整 HTML
  - 即 `innerHTML` 加上元素本身
- `textContent`：获取元素中的文本内容
- `nodeValue/data`：获取非元素节点的文本内容

> 设置 innerHTML 和 textContent 的区别
>
> - 使用 innerHTML 会将其作为 HTML 插入，其中的元素内容会被浏览器解析
> - 使用 textContent  会将其作为文本插入，其中的元素内容会被当成文本的一部分

## 元素属性

一个元素除了有开始标签、结束标签、内容之外，还有很多的属性（attribute）

浏览器在解析 HTML 元素时，会将对应的 attribute 也创建出来放到对应的元素对象上

元素的全局属性

- `hidden` ：设置元素隐藏
- `id`
- `class`
- `title`
- `style`

元素的特殊属性

- `type`：input 的类型

- `value`：表单元素的值
- `href`：a 元素的 href

### attribute

attribute 分类

- 标准 attribute：某些 attribute 是标准的
- 非标准 attribute：还有一些 attribute 是自定义的

attribute 操作

- `elem.hasAttribute(name)`：检查是否存在
- `elem.getAttribute(name)`：获取
- `elem.setAttribute(name, value)`：设置
- `elem.removeAttribute(name)`：移除
- `elem.attributes`：包含所有 attr 对象的集合，具有 name、value属性

attribute 的特征

- 它们的名字是大小写不敏感的（id 与 ID 相同）
- 它们的值总是字符串类型的

### property

对于标准的 attribute，会在 DOM 对象上创建与其对应的 property

`elem.id`

`elem.className`

`elem.abc // undefined`

在大多数情况下，它们是相互作用的

- 改变 property，通过 attribute 获取的值，会随着改变

- 通过 attribute 操作修改，property 的值会随着改变

  但是部分浏览器中 input 的 value 修改只能通过 attribute 的方法

除非特别情况，大多数情况下，设置、获取 attribute，推荐使用 property 的方式

因为 property 在默认情况下是有类型的

比如 `inputEl.getAttribute("checked")` 返回的是字符串，而 `inputEl.checked` 返回的是布尔值

### dataset

元素的 dataset 属性中可以获取到用 `data-*` 自定义的属性

```js
<div class="box" title="abc" data-name="why" data-age="18">
  box
</div>
<script>
  const box = document.querySelector(".box")
  console.log(box.dataset.name)
  console.log(box.dataset.age)
</script>
```

## 类和样式

动态修改样式的两种方法

1. 动态的添加和移除 class
2. 动态的修改元素的 style 属性

### className

元素的 class attribute 对应的 DOM 中的 property 叫做 className

className 可以用于获取和修改元素的 class 属性

className 对应的所有 class 保存在 classList 里

### classList

classList 是可迭代对象，可以通过 `for of` 遍历

classList 对象提供了一些方法对类进行操作

- `elem.classList.add(class)`：添加类
- `elem.classList.remove(class)`：移除类
- `elem.classList.toggle(class)`：如果类不存在就添加类，存在就移除类
- `elem.classList.contains(class)`：检查给定类是否存在

### style

- style 可以用于单独修改某一个 CSS 属性

  `elem.style.width`

- 多词属性（multi-word）使用驼峰式 camelCase

  `elem.style.backgroundColor`

- 将 CSS 属性设置为空字符串会使用 CSS 默认样式

  `elem.style.display = ""`

- 同时设置多个样式

  `elem.style.cssText = "font-size: 30px; color: red;"`

  cssText 会覆盖元素的 style 属性，因此不推荐使用

- 读取样式

  内联样式可以直接通过 `elem.style` 对象读取其中的 CSS 属性

  但对于 `<style></style>` 和 CSS 文件中的样式并不生效

  全局函数 `getComputedStyle(elem)` 可以获取到元素的样式

  ```js
  getComputedStyle(elem).width
  getComputedStyle(elem).height
  getComputedStyle(elem).backgroundColor
  ```

## 元素操作

### 创建元素

早期没有 DOM 时一般使用 `document.wirte()` 写入一个元素

目前创建元素的常用步骤

1. 创建一个元素

   `document.createElement(tag)`

   不推荐直接用 `innerHTML`，因为后续操作还需要再找到新的元素

   ```js
   var boxEl = document.querySelector(".box")
   var h2El = document.createElement("h2")
   h2El.innerHTML = "我是标题"
   h2El.classList.add("title")
   boxEl.append(h2El)

2. 插入元素到 DOM 的某一个位置

   - `node.append(...nodes or strings)` —— 在 node 末尾插入节点或字符串

   - `node.prepend(...nodes or strings)` —— 在 node 开头插入节点或字符串

   - `node.before(...nodes or strings)` —— 在 node 前面插入节点或字符串

   - `node.after(...nodes or strings)` —— 在 node 后面插入节点或字符串

   - `node.replaceWith(...nodes or strings)` —— 将 node 替换为给定的节点或字符串

   ![](../images/node-insert.png)

### 移除元素

移除元素我们可以调用元素本身的 `remove()`

`elem.remove()`

### 克隆元素

`elem.cloneNode(deep)` 可以复制一个现有的元素

- 可以传入一个 Boolean 类型的值 deep，来决定是否深度克隆
- 深度克隆会克隆对应元素的子元素，否则不会克隆子元素

### 旧操作

- `parentElem.appendChild(node)`：在 parentElem 即父元素最后位置添加一个子元素
- `parentElem.insertBefore(node, nextSibling)`：在 parentElem 的 nextSibling 前面插入一个子元素
- `parentElem.replaceChild(node, oldChild)`：在 parentElem 中，用新元素替换之前的 oldChild 元素
- ` parentElem.removeChild(node)`：在 parentElem 中，移除某一个元素

## 元素尺寸

<img src="../images/element-size.png" style="zoom: 67%;" />

### 偏移尺寸

- `offsetWidth`：元素完整的宽度
- `offsetHeight`：元素完整的高度
- `offsetLeft`：距离父元素的 x
- `offsetTop`：距离父元素的 y

<img src="../images/offset-dim.png" style="zoom: 80%;" />

### 客户端尺寸

- `clientWidth`：contentWidth + 左右padding（不含滚动条）
- `clientHeight`：contentHeight + 上下padding（不含滚动条）
- `clientLeft`：border-left 的宽度
- `clientTop`：border-top 的宽度

<img src="../images/client-dim.png" style="zoom: 80%;" />

contentWith 的计算

- `box-sizing: content-box;`时 contentWith = width - scrollbar
- `box-sizing: border-box;`时 contentWith = width - padding - border - scrollbar

客户端尺寸指的是元素内部的空间，因此不包含滚动条占用的空间

常用于确定浏览器视口尺寸

- `document.documentElement.clientWidth`
- `document.documentElement.clientHeight`

### 滚动尺寸

- `scrollWidth`：整个可滚动的区域宽度
- `scrollHeight`：整个可滚动的区域高度
- `scrollTop`：滚动部分的高度
- `scrollLeft`：滚动部分的宽度

<img src="../images/scroll-dim.png" style="zoom: 80%;" />

## 定时器

有时我们并不想立即执行一个函数，而是等待特定一段时间之后再执行，我们称之为**计划调用（scheduling a call）**

实现计划调用的两种方式

- `setTimeout`：允许我们将函数推迟到一段时间间隔之后再执行
- `setInterval`：允许我们重复运行一个函数，从一段时间间隔之后开始运行，之后以该时间间隔连续重复运行该函数

对应的取消方法

- `clearTimeout`：取消 setTimeout 的定时器
- `clearInterval`：取消 setInterval 的定时器

### 定时执行

`setTimeout` 的语法如下

`let timerId = setTimeout(func|code, [delay], [arg1], [arg2], ...)`

- func|code：想要执行的函数或代码字符串

   一般传入的都是函数，由于某些历史原因，支持传入代码字符串，但是不建议这样做

- delay：执行前的延时，以毫秒为单位（1000 毫秒 = 1 秒），默认值是 0
- arg1，arg2…：要传入被执行函数（或代码字符串）的参数列表

`clearTimeout` 的使用

setTimeout 在调用时会返回一个定时器标识符（timer identifier）

我们可以使用它来取消执行 `clearTimeout(timerID)`

### 循环执行

`setInterval` 和 `setTimeout` 的语法相同

`let timerId = setInterval(func|code, [delay], [arg1], [arg2], ...)`

`setInterval`  参数含义与 `setTimeout` 相同

`clearInterval` 的使用

setInterval 也会返回一个定时器标识符（timer identifier）

我们可以通过clearInterval 来取消这个定时器 `clearInterval(timerID)`

# BOM

## 理解 BOM

BOM 浏览器对象模型

- BOM 包含由浏览器提供的用于处理文档之外的所有内容的其他对象

- 浏览器是 JS 的运行环境，同时又作为一个需要对其操作的应用程序

  因此浏览器通过了一些相关对象模型用于连接 JS 与浏览器窗口

- BOM 主要包含以下对象模型

  - window：包括全局属性、方法，控制浏览器窗口相关的属性、方法
  - location：浏览器连接到的对象的位置（URL）
  - history：操作浏览器的历史
  - navigator：用户代理（浏览器）的状态和标识（很少用到）
  - screen：屏幕窗口信息（很少用到）

## window 对象

window 对象在浏览器中的两个视角

- 全局对象：提供一些全局属性、全局函数和类
  - node 中的全局对象是 global，目前已统一标准为 globalThis，大多数现代浏览器都支持
  - 放在 window 对象上的所有属性都可以被访问
  - 使用 var 定义的变量会被添加到 window 对象中
- 浏览器窗口对象：提供对浏览器操作的相关 API

[window 对象](https://developer.mozilla.org/zh-CN/docs/Web/API/Window)的作用

- 提供了大量的属性

- 提供了大量的方法

- 提供了大量的事件

- 包含了从 EventTarget 继承过来的方法（Window 类也是继承自 EventTarget 的）

  - `addEventListener`
  - `removeEventListener`
  - `dispatchEvent`

### 常见属性

- `window.innerWidth / window.innerHeight`：浏览器窗口中页面视口的大小（不包含浏览器边框和工具栏）。
- `window.outerWidth / window.outerHeight`：浏览器窗口自身的大小
- `window.scrollX / window.pageXOffset`：文档相对于视口在 X 轴上的滚动距离
- `window.scrollY / window.pageYOffset`：文档相对于视口在 Y 轴上的滚动距离

### 常见方法

- `scrollBy(x, y)`：将页面滚动至相对于当前位置的 `(x, y)` 位置

- `scrollTo(pageX, pageY)`：将页面滚动至绝对坐标  `(pageX, pageY)`

- `open()`：打开一个新窗口

- `close()`：关闭当前窗口或某个指定的窗口

  该方法只能由 open() 方法打开的窗口的 window 对象来调用

### 常见事件

- `onfocus`：窗口获取到焦点
- `onblur`：窗口失去焦点
- `onload`：页面加载完成
- `onhashchange`：hash 值发生改变

## location 对象

location 对象用于表示 window 上当前链接到的 URL 信息

![](../images/url.png)

location 其实是 URL 的一个抽象对象

### 常见属性

- `href`：当前 window 对应的超链接 URL，整个 URL
- `protocol`：当前的协议
- `host`：主机地址
- `hostname`：主机地址（不带端口）
- `port`：端口
- `pathname`：路径
- `search`：查询字符串（带问号）
- `hash`：哈希值
- `username`：URL 中的 username（很多浏览器已经禁用）
- `password`：URL 中的 password（很多浏览器已经禁用）

### 常见方法

- `assign()`：赋值一个新的 URL，并且跳转到该 URL 中

- `replace()`：打开一个新的 URL，并且跳转到该 URL 中

  不同的是不会在浏览记录中留下之前的记录（不能返回）

- `reload()`：重新加载页面，可以传入一个 Boolean 类型

### URLSearchParams

[URLSearchParams](https://developer.mozilla.org/zh-CN/docs/Web/API/URLSearchParams) 定义了一些实用的方法来处理 URL 的查询字符串

- 将字符串转换为 URLSearchParams 类型

  ```js
  var urlsearch = new URLSearchParams("name=me&age=18")
  console.log(urlsearch.get("name"))
  ```

- 将 URLSearchParams 类型转换为字符串

  ```js
  var urlStr = urlsearch.toString()
  ```

- URLSearchParams 常见的方法
  - `get`：获取搜索参数的值
  - `set`：设置一个搜索参数和值
  - `append`：追加一个搜索参数和值
  - `has`：判断是否有某个搜索参数
- 中文会使用 encodeURIComponent 和 decodeURIComponent 进行编码和解码

## history 对象

history 对象允许我们访问浏览器曾经的会话历史记录

history 和 hash 目前是 vue、react 等框架实现路由的底层原理

前端路由核心：修改 URL，但是页面不刷新

可以通过修改 hash 和修改 history 来实现修改 URL 而页面不刷新的效果

hash 发生改变的时候不会刷新整个页面，所以监听 hash 的改变就可以动态渲染部分页面

### 常见属性

- `length`：会话中的记录条数
- `state`：当前保留的状态值

### 常见方法

- `back()`：返回上一页，等价于 `history.go(-1)`

- `forward()`：前进下一页，等价于 `history.go(1)`

- `go()`：加载历史中的某一页

- `pushState(state, unused, url)`：增加一条新的历史记录条目

  - state：JS 对象（可为空）

    导航到新的 state 会触发 `popstate` 事件

    且该事件的 `state` 属性包含 `history.state` 的副本

    调用 `history.pushState()` 或者 `history.replaceState()` 不会触发 `popstate` 事件

    `popstate` 事件只会在浏览器某些行为下触发

    比如点击后退按钮或者在 JavaScript 中调用 `history.back()` 方法

    即在同一文档的两个历史记录条目之间导航会触发该事件

  - unused：页面标题，由于历史原因，该参数存在且不能忽略

    传递一个空字符串是安全的，以防将来对该方法进行更改

  - url：新历史条目的 URL，可以是当前 URL 的相对路径，必须同源不能跨域

    没有指定则设置为当前文档的URL

    如果当前 url 为 `http://127.0.0.1:5500/JavaScript/pratice/bom-history.html`

    - `/userlist` 是绝对路径，会跳转到 `http://127.0.0.1:5500/userlist`
    
    - `userlist` 是相对路径，会跳转到 `http://127.0.0.1:5500/JavaScript/pratice/userlist`

- `replaceState()`：替换当前的历史记录条目

  与 `pushState()` 的使用相同，但是不能返回

## 不常用对象

### navigator 对象

navigator 对象表示用户代理的状态和标识等信息

比如用于检测浏览器的 `navigator.userAgent`

[Navigator - Web API 接口参考 | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/API/Navigator)

### screen 对象

screen 主要记录的是浏览器窗口外面的客户端显示器的信息

比如屏幕的逻辑像素 screen.width、screen.height

[Screen - Web API 接口参考 | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/API/Screen)
