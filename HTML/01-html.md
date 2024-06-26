# HTML 基础知识

## 网页和网站

1. 什么是网页

   - 网页的专业术语叫 **Web Page**
   - 打开浏览器查看到的页面
   - 网页的内容可以非常丰富，包括文字、链接、图片、音乐、视频等等

2. 网站是由多个网页组成的

3. 网页的显示过程

   - 用户角度

     1. 在浏览器输入一个网站
     2. 浏览器会找到对应的服务器地址，请求静态资源
     3. 服务器返回静态资源给浏览器
     4. 浏览器对静态资源进行解析和展示

   - 前端工程师角度

     1. 开发项目（HTML/CSS/JavaScript/Vue/React）
     2. 打包、部署项目到服务器里面

4. 网页的组成部分

   - HTML：网页的内容结构（骨架）
   - CSS：网页的视觉体验（外表）
   - JS：网页的交互处理（灵魂）

## 浏览器

1. 浏览器的作用是**渲染网页**

2. 浏览器最核心的部分是**渲染引擎**，一般也称为**浏览器内核**

   浏览器内核负责解析网页语法，并渲染网页

3. 常见的浏览器内核
   - Trident（三叉戟）：IE、360安全、搜狗、百度、UC；
   - Gecko（壁虎 ）：Firefox
   - Presto → Blink：Opera
   - Webkit：Safari、360极速、搜狗、移动端浏览器
   - Webkit → Blink（眨眼）：Google Chrome、Edge

4. 不同的浏览器内核有不同的解析、渲染规则

   所以同一网页在不同内核的浏览器中的渲染效果也可能不同

## HTML 语言

1. HTML 定义

   HTML 全称为 Hyper Text Markup Language

   即**超文本标记语言**，是一种用于创建网页的标记语言

2. 什么是标记语言

   - 标记语言由无数个标签组成

   - 标签的作用是对某些内容进行特殊的标记，以供其他解释器识别处理

     比如 h2 标记的文本会被识别为标题以加粗和放大显示文本

3. 什么是元素

   由标签和内容组成的部分称为元素

4. 什么是超文本
   - 不仅仅可以插入普通的文本，还可以插入图片、音频、视频等内容
   - 还可以表示超链接，从一个网页跳转到另一个网页

## HTML 结构

1. HTML 文件的特点是他们都具有共同的结构

2. HTML 文件的拓展名是 .htm 和 .html

   因历史遗留问题，Win95 和 Win98系统的文件拓展名不能超过3字符，所以使用 .htm

3. HTML 文件本质上是由一系列的 **HTML 元素**构成的

4. 完整的 HTML 结构

   - 文档声明：
     
     HTML 文件最上方的一段文本 `<!DOCTYPE html>` 就是它的文档类型声明，用于声明文档类型
     
     - 告诉浏览器当前页面是 HTML5 页面
     - 让浏览器用 HTML5 的标准去解析识别内容
     - 必须放在 HTML 文档的最前面，不能省略，省略会出现兼容问题
     
   - html 元素
     - head 元素
     - body 元素

## HTML 注释

1. 什么是注释

   `<!-- 注释内容 --> `

   注释是帮助开发者理解代码的一段代码说明，浏览器并不会把注释显示给用户看

2. 注释的意义

   - 帮助我们理清代码思路，方便以后查阅
   - 减少合作开发的沟通成本
   - 方便别人使用和学习
   - 临时注释代码，方便调试

## HTML 元素

### 元素的概念

1. 什么是元素

   ![img](../images/element.png)

   - 元素是网页的一部分
   - 一个元素可以包含一个数据项，一个文本，或者一张照片，亦或是什么也不包含

2. [HTML 元素参考](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element)

3. 元素的组成部分：

   以 `<p>"content"</p>` 为例

   - 开始标签（Opening tag）：`<p>`
   - 结束标签（Closing tag）：`</p>`
   - 内容（Content）：`"content"`
   - 属性（Attribute）：`class="nice"`

4. 元素可以分为双标签元素和单标签元素

   - 大部分元素都是双标签的

     `html, body, head, h2, p, a`

   - 也有一些元素是单标签的

     `br, img, hr, meta, input`

5. HTML 元素不区分大小写， 但是**推荐小写**

### 元素的属性

`<p class="ele-con">"content"</p>`

1. 属性包含元素的额外信息，这些信息不会出现在实际的内容中

2. 属性主要由属性的名称和属性值组成

   `class="ele-con"`

   `href="http://www.baidu.com"`

3. 属性与属性之间要用空格分隔开

4. 元素属性的分类

   - [全局属性](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Global_attributes)
     - id：定义元素的唯一表示符
     - class：一个以空格分隔的元素的类名列表
     - style：给元素添加内联样式
     - title：包含表示与其所属元素相关信息的文本，这些信息通常可以作为提示呈现给用户，但不是必须的
   - 特有属性
     - meta 元素的 charset 属性
     - img 元素的 src、alt 属性
     - a 元素的 href 属性

### 元素的嵌套

某些元素的内容除了可以是文本之外，还可以是其他元素，这样就形成了**元素的嵌套**

元素之间的关系分为
- 父子关系
- 兄弟关系
- 后代关系

### 元素的类型

HTML 在设计元素时根据元素理论上需要占据的空间将元素分为以下几种

1. 块级元素：独占父元素的一行

   可以设置宽度和高度

2. 行内级元素：多个行内级元素可以在父元素的同一行显示

   - 行内替换元素：可以设置宽高（如 img 元素、input 元素）
   - 行内非替换元素：不可以设置宽高，宽高是由内容决定（如 a 元素）


### 元素编写的注意事项

- 块级元素和 inline-block 元素
  - 一般情况下，可以包含其他任何 元素
  - 特殊情况，p 元素不能包含其他块级元素
- 行内级元素
  - 一般情况，只能包含行内级元素

### HTML结构中的常见元素

1. html 元素

   - html 元素表示一个 HTML 文档的根元素（顶级元素）

   - 所有其他元素都是此元素的后代

   - W3C 标准建议为 html 元素增加一个 lang 属性

     `lang=en` `lang=zh-CN`

     lang 属性的作用是
     
     - 帮助语音合成工具确定要使用的发音
     - 帮助翻译工具确定要使用的翻译规则

2. head 元素

   - 用于规定文档相关的配置信息（也称为元数据）

     包括文档的标题，引用的文档样式和脚本等

     元数据就是描述数据的数据，可以理解成对整个页面的配置

   - title 元素

     `<title>网页的标题</title>`

     用于设置网页的标题

   - meta 元素

     meta 元素表示那些不能由 HTML 元相关元素（meta-related elements）表示的元数据信息

     - charset：用于设置网页的字符编码

       `<meta charset="utf-8">` 用于设置网页的字符编码，一般使用 utf-8 编码

       让浏览器更精准地显示每一个文字，不设置或者设置错误会导致乱码

     - http-equiv：编译指令

     - name：提供文档级别的元数据，应用于整个页面

       [标准元数据名称](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/meta/name)
       
       - author
       - Copyright
       - description
       - keywords

   - link 元素

     `<link rel="stylesheet" href="example.css">`

     用于链接外部资源，如网站图标、CSS 样式表等等
     
     - link 元素是**外部资源链接元素**，规范了**文档与外部资源**的关系
     - link 元素通常位于 head 元素中
     - link 最常用于链接 CSS 样式表，此外也可以被用来创建站点图标
     - link 元素常见的属性
       - href：此属性用于指定被链接资源的 URL，URL 可以是绝对的，也可以是相对的
       - [rel](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Attributes/rel)：指定链接类型
         - `icon` 用于站点图标
         - `stylesheet` 用于 CSS 样式表
         - `dns-prefetch` 用于告知浏览器为目标资源的来源预先执行 DNS 解析

3. body 元素

   用于呈现网页的具体内容和结构

### HTML内容中的常见元素

1. h 元素（heading）

   - 用于设置一些比较重要的文字作为标题
   - h1-h6 呈现了六个不同级别的标题
   - h 元素通常和 SEO 优化有关系

2. p 元素（paragraph）

   - 用于表示文本的一个段落
   - p 元素多个段落之间会有一定的默认间距

3. img 元素（image）

   - 用于将一份图像嵌入文档

   - img 是一个可替换元素（replaced element）

   - img 的两个常见属性

     - src：必须属性

       表示图片的文件路径

     - alt：非必须属性

       作用一：当图片加载不成功时显示这段文本

       作用二：屏幕阅读器会将这些描述读给需要使用阅读器的使用者听，让他们知道图像的含义

     - 某些其他属性目前已经不再使用，比如 width、height、border

   - img 的图片路径

     - 网络图片：src 可以是一张网络图片的 URL 地址

     - 本地图片：src 也可以是本地图片的路径

       本地图片的路径有两种：
       
       一、绝对（absolute）路径：从电脑根目录开始查找直到找到这个资源
       
       二、相对（relative）路径：图片相对于当前文件的路径

   - [img支持的图像格式](https://developer.mozilla.org/zh-CN/docs/Web/Media/Formats/Image_types)

4. a 元素（anchor）

   - 用于定义超链接，以打开新的 URL

   - a 元素的两个常见属性

     - href：Hypertext Reference 的简称
       - 指定要打开的URL地址
       - 也可以是一个本地地址
     - target：指定在何处显示链接的资源
       - `_self`：默认值，在当前窗口打开 URL
       - `_blank`：在一个新的窗口打开 URL
       - `_parent`：在父级窗口打开 URL
       - `_top`：在顶层窗口打开 URL

   - a 元素也可以作为锚点链接，用于跳转到网页中的具体位置

     - 先在要跳转的元素上定义一个id属性

       `id="title"`

     - 定义 a 元素，并且 a 元素的 href 指向对应的 id

       `<a href="#title">jump to title</a>`

   - a 元素和 img 元素的结合使用

     ```HTML
     <a href="xxx.com" target="_blank">
       <img src="" alt="">
     </a>
     ```
   
   - a 元素和其他 URL 的结合
       - 下载文件
   
           `href="http://xxx/xxx.zip"`
       - 指向其他协议地址如 mailto
   
           `href="mailto:myemail@qq.com"`
   
5. iframe 元素

    - 用于在一个 HTML 文档中嵌入其他 HTML 文档

      `<iframe src="http://www.taobao.com" frameborder="0"></iframe>`

    - frameborder 属性用于规定是否显示边框

      - 1：显示
      - 0：不显示

    - a 元素 target 的部分值与 iframe 有关

      - `_parent`：在父级窗口打开URL
      
      - `_top`：在顶层窗口打开URL
      

6. div 元素（division）和 span 元素

   - div 和 span 的出现主要是用于编写 HTML 的结构
   
   
   - div 和 span 都是纯粹的容器，也可以把他们理解为用来包裹内容的盒子
   
   - div 用于把网页分割为多个独立的部分
   
     多个 div 元素包裹的内容会在不同的行显示，一般作为其他元素的父容器，把其他元素包住，代表一个整体
   
   - span 用于区分特殊文本和普通文本，比如用来显示一些关键字
   
     多个 span 元素包裹的内容会在同一行显示，默认情况下，跟普通文本几乎没差别
   
   > **div 和 span 的比较**
   >
   > 相同点：
   >
   > 1. 都是用于编写 HTML 结构的元素
   > 2. 都是单纯的包裹内容的容器，没有固定语义
   >
   > 不同点
   >
   > 1. div 用于分割网页的不同部分，span 用于区分特殊文本
   > 2. 多个 div 元素包裹的内容会在不同的行显示，多个 span 元素包裹的内容会在同一行显示


7. 不常用元素

    - strong：加粗，强调内容
    
    - i：内容倾斜
    
    - code：显示代码
    - br：换行

## 高级元素

### 列表元素

很多数据在网页中都是以列表的形式存在的

**列表的两种实现方式**

1. 使用 div 来实现，样式和布局更加自由
2. 使用列表元素来实现，更符合元素语义化

**常用的列表元素**

- ol：有序列表

  直接子元素只能是 li

  - li：列表项

- ul：无序列表

  直接子元素只能是 li

  - li：列表项

- dl：定义列表

  直接子元素只能是 dt、dd

  一个dt 后面一般紧跟着一个或者多个 dd

  - dt：定义项
  - dd：定义描述

### 表格元素

在网页中，对于某些内容的展示使用表格元素更为合适和方便

**常用的表格元素**

- table：表格
- tr：表格中的行
- td：行中的单元格

表格有很多相关的属性可以设置表格的样式，但是已经不推荐使用了

**border-collapse**是用来决定表格的边框是分开还是合并的 CSS 属性

```css
table {
  border-collapse: collapse;
}
```

**表格的其他元素**

- 表格的表头 thead
- 表格的主体 tbody
- 表格的页脚 tfoot
- 表格的标题 caption
- 表格的表头单元格 th

**单元格合并**

- 跨列合并 colspan

  在最左边的单元格写上 colspan 属性，并且省略掉合并的 td

- 跨行合并 rowspan

  在最上边的单元格写上 rowspan 属性，并且省略掉后面 tr 中的 td

**表格内部元素的编写顺序**

1. 一个可选的 `<caption>` 元素
2. 零个或多个 `<colgroup>` 元素
3. 一个可选的 `<thead>` 元素
4. 零个或多个 `<tbody>` 元素
5. 一个可选的 `<tfoot>` 元素

### 表单元素

表单元素是网站和用户交互的重要方式之一，在很多网站都需要使用表单

**常见的表单元素**

- form：表单
- input：输入框
- textarea：多行文本框
- select、option：下拉选择框
- button：按钮
- label：表单元素的标题

**form 的常见属性**

form 通常作为表单元素的父元素，可以将整个表单作为一个整体来进行操作

比如对整个表单重置或者提交整个表单的数据

- action：用于设置提交表单数据的请求URL
- method：用于设置请求的方法（get 和 post），默认为 get
- target：用于设置在什么地方打开 URL
  - `_blank` ：新开页面
  - `_self` ：当前页面跳转
  - `_parent`： 当前父级页面跳转
  - `_top`： 当前顶层页面跳转

**[input](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/Input) 元素的常见属性**

- type：输入类型

  `text / password / radio / checkbox / button / reset / submit / file`

- readonly：是否只读

- disabled：是否禁用

- checked：是否默认被选中

  只有在 type 为 radio 和 checkbox 时可用

- autofocus：当页面加载时自动聚焦

- name：名字

  在提交数据给服务器时，可用于区分数据类型

  name 对应后台接收数据时使用的键值对中的键（key） 

  会随着表单的提交而一起提交，是表单中不可或缺的元素

  一个 form 表单中该元素的名称对应不同类型的 input

- value：取值

  value 对应后台接收数据时使用的键值对中的值（value） 

  value 可以有默认值

> **布尔属性**
>
> 常见的布尔属性有：disabled、checked、readonly、multiple、autofoucs、selected
>
> 布尔属性可以没有属性值，写上属性名就代表使用这个属性

**button 按钮**

- 普通按钮：type=button

  使用value属性设置按钮文字

- 重置按钮：type=reset

  重置它所属表单的所有表单元素（包括input、textarea、select）

- 提交按钮：type=submit

  提交它所属form的表单数据给服务器（包括input、textarea、select）

```html
<input type="button" value="普通按钮">
<input type="reset" value="重置按钮">
<input type="submit" value="提交按钮">
也可以通过button来实现
<button type="button">普通按钮</button>
<button type="reset">重置按钮</button>
<button type="submit">提交按钮</button>
```

**label 元素**

- label 元素一般与 input 配合使用，用来表示 input 的标题

- label 可以跟某个 input 绑定

  点击 label 就可以激活对应的 input 变成选中状态

  label 的 for 属性和 input 的 id 属性相等时绑定

```html
<div>
  <label for="username">用户：</label>
  <input id="username" type="text" name="username">
</div>
```

**radio 的使用**

- 使用 label 可以实现点击文字选中单选框
- name 值相同的 radio 才具备单选功能

```html
<div>
  性别：
  <label for="male">
    男 <input id="male" type="radio" name="sex" value="male">
  </label>
  <label for="female">
    女 <input id="female" type="radio" name="sex" value="female">
  </label>
</div>
```

**checkbox 的使用**

属于同一种类型的 checkbox，name 值要保持一致

```html
<div>
  爱好：
  <label for="basketball">
    <input id="basketball" type="checkbox" name="hobby" value="basketball">篮球
  </label>
  <label for="football">
    <input id="football" type="checkbox" name="hobby" value="football">足球
  </label>
  <label for="reading">
    <input id="reading" type="checkbox" name="hobby" value="reading">读书
  </label>
</div>
```

**textarea 的使用**

- 常用属性
  - cols：列数
  - rows：行数
- 通过 CSS 设置 textarea 的缩放类型
  - 禁止缩放：`resize: none;`
  - 水平缩放：`resize: horizontal;`
  - 垂直缩放：`resize: vertical;`
  - 水平垂直缩放：`resize: both;`

**select 和 option 的使用**

- option 是 select 的子元素，一个 option 代表一个选项
- select 的常用属性
  - multiple：可多选
  - size：显示多少项
  - name
- option 的常用属性
  - selected：默认被选中
  - value
