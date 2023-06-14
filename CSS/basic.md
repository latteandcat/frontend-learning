# CSS基础知识

## CSS概念

### CSS定义

1. CSS（Cascading Style Sheet）表示层叠样式表，用于为网页添加样式

2. CSS语言是一种计算机语言，但不算是一种编程语言

3. CSS的出现是为了美化HTML的，并且让结构（HTML）与样式（CSS）分离

   - 美化方式一：为HTML添加各种各样的样式，比如颜色、字体、大小、下划线等等
   
   
      - 美化方式二：对HTML进行布局，按照某种结构显示
   


### CSS语法规则

声明（Declaration）一个单独的CSS规则，用来添加指定的CSS样式

`声明 = 属性名: 属性值;`

- 属性名（Property name）：要添加的css规则的名称
- 属性值（Property value）：要添加的css规则的值

### CSS的使用方法

1. 内联样式（inline style）

   内联样式存在于HTML元素的style属性之中

   `<div style="color: red;"></div>`

2. 内部样式表（internal style sheey）

   将CSS放在HTML文件`<head>`元素里的`<style>`元素之中

3. 外部样式表（external style sheet）

   将CSS编写在一个独立的文件中，然后通过`<link>`元素引入进来

   `<link rel="stylesheet" href="./css/style.css">`

   此外可以在style元素或者CSS文件中使用`@import`导入其他的元素

   `@imort url(./other.css);`

### CSS注释

- CSS代码也可以添加注释来方便阅读
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
