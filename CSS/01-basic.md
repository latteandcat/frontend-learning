# CSS 基础知识

## CSS 概念

### CSS 定义

1. CSS（Cascading Style Sheet）表示层叠样式表，用于为网页添加样式

2. CSS 语言是一种计算机语言，但不算是一种编程语言

3. CSS 的出现是为了美化 HTML 的，并且让结构（HTML）与样式（CSS）分离

   - 美化方式一：为 HTML 添加各种各样的样式，比如颜色、字体、大小、下划线等等
   
   
      - 美化方式二：对 HTML 进行布局，按照某种结构显示
   


### CSS 语法规则

声明（Declaration）是一个单独的 CSS 规则，用来添加指定的 CSS 样式

`声明 = 属性名: 属性值;`

- 属性名（Property name）：要添加的 CSS 规则的名称
- 属性值（Property value）：要添加的 CSS 规则的值

### CSS 的使用方法

1. 内联样式（inline style）

   内联样式存在于 HTML 元素的 style 属性之中

   `<div style="color: red;"></div>`

2. 内部样式表（internal style sheet）

   将 CSS 放在 HTML 文件 `<head>` 元素里的 `<style>` 元素之中

3. 外部样式表（external style sheet）

   将 CSS 编写在一个独立的文件中，然后通过 `<link>` 元素引入进来

   `<link rel="stylesheet" href="./css/style.css">`

   此外可以在 style 元素或者 CSS 文件中使用 `@import` 导入其他的元素

   `@imort url(./other.css);`

### CSS 注释

- CSS 代码也可以添加注释来方便阅读
- 注释的方法 `/* */`

## CSS属性

### 常用属性

- `font-size`：文字大小
- `color`：前景色
- `background-color`：背景色
- `width`：宽度
- `height`：高度

### 相关链接

[官方文档地址（W3C）](https://www.w3.org/TR/?tag=css)

[推荐文档地址（MDN）](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Reference#关键字索引)

[查询CSS属性的可用性](https://caniuse.com/)
