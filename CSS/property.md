# CSS属性

## CSS特性

### 属性继承

CSS的某些属性具有继承性

- 如果一个属性具备继承性，那么在该元素上设置后，它的后代元素都可以继承这个属性

- 如果后代元素自己也设置了该属性，那么优先使用后代元素自己的属性（不管继承过来的属性权重多高）

- 常见的`font-size/font-family/font-weight/line-height/color/text-align`都具有继承性

  可以从MDN文档中查阅每个属性的继承性

- 继承过来的是计算值，而不是设置值

  2em继承到的是计算后的px而不是em 

- 强制继承

  ```css
  .box {
    border: 1px solid purple;
  }
  
  .box p {
    border: inherit;
  }
  
  ```

### 属性层叠

对于一个元素来说，一个相同的属性可以通过不同的选择器进行多次设置，此时属性会被一层层覆盖到元素上，但是最终只有一层会生效

判断生效属性的方式

1. 根据选择器的权重判断：权重较大的选择器优先生效，根据选择器的权重判断出优先级
2. 根据先后顺序判断：选择器的权重相同时，后设置的生效

选择器的权重

- !important：10000
- 内联样式：1000
- id选择器：100
- 类选择器、属性选择器、伪类：10
- 元素选择器、伪元素：1
- 通配符：0

## 特殊属性

### display

display用于修改元素的显示类型

- block：让元素显示为块级元素

  - 独占一行
  - 可以设置宽高
  - 默认高度由内容决定

- inline：让元素显示为行内级元素

  - 非独占一行
  - 不可以设置宽高
  - 默认宽高由内容决定

- inline-block：让元素同时具备行内级元素和块级元素的特征

  - 非独占一行
  - 不可以设置宽高
  - 宽度和高度由内容决定

  > inline-block可以这样理解：对外来说，它是一个行内级元素；对内来说，它是一个块级元素

- none：隐藏元素

## 文本属性

### text-decoration⭐

`text-decoration`用于设置文本的装饰线

- `none`：无任何装饰线

  可以用于去除a元素默认的下划线

- `underline`：下划线

- `overline`：上划线

- `line-through`：中划线（删除线）

`color`属性也会改变装饰线的颜色

### text-transform

`text-transform`用于设置文本的大小写转换

- `capitalize`：将每个单词的首字符变为大写
- `uppercase`：将每个单词的所有字符变为大写
- `lowercase`：将每个单词的所有字符变为小写
- `none`：没有任何影响

### text-indent

`text-indent`用于设置第一行内容的缩进

`text-indent: 2em`刚好是缩进两个文字

### text-align⭐

`text-align`用于设置文本的对齐方式

本质是定义行内内容（文字、图片）如何相对它的块父元素对齐

> W3C中的解释：
>
> This shorthand property sets the 'text-align-all' and the 'text-align-last' properties and describes how the **inline-level content of a block** is aligned along the inline axis if the content does not completely fill.

- `left`：左对齐

- `right`：右对齐

- `center`：居中对齐

- `justify`：两端对齐

  `text-align: justify;`最后一行是无效的，可以通过`text-align-last: justify;`使最后一行也变成两端对齐

### word/letter-spacing

`word-spacing`、`letter-spacing`分别用于设置**单词、字母**之间的间距

## 字体属性

### font-size

`font-size`用于设置文字的大小

- 具体数值+单位：比如100px、2em

  1em=父元素的font-size

- 百分比：基于父元素的`font-size`计算

### font-family

`font-family`用于设置文字的字体名称

- 可以设置一个或者多个文字的字体名称
- 浏览器会选择列表中第一个该计算上有安装的字体
- 或者是通过`@font-face`指定的可以直接下载的字体

### font-weight

`font-weight`用于设置文字的粗细（重量）

- `number`：1|100|200|...|900|1000
- `normal`：400
- `bold`：700
- `lighter`：比从父元素继承来的值更细
- `bolder`：比从父元素继承来的值更粗

strong、b、h1-h6等标签的font-weight默认就是bold

### line-height⭐

`line-height`用于设置文本的行高，行高即一行文字所占据的高度，行高的作用是提高文本的阅读体验

行高的严格定义是：两行文字基线（baseline）之间的间距，基线是与小写字母最底部对齐的线 

![](../images/baseline.png)

行距 = line-height 减 文本的高度

行距是上下等分的，所以通过使`height`的值和`line-height`的值相等可以实现文本的垂直居中，但只能对文本使用

### font⭐

`font`是一个缩写属性

可以作为`font-style`、`font-variant`、`font-weight`、`font-size`、`line-height`和`font-family`的缩写

`font: font-style font-variant font-weight font-size/line-height font-family`

规则：

- `font-style`、`font-variant`、`font-weight`可以随意调换顺序，也可以省略

- `/line-height`可以省略，如果不省略必须放在`font-size`后面

  在`font`中`line-height=1.5`是相对于`font-size`的

- `font-size`和`font-family`不可以调换顺序，不可以省略

### fony-style

`font-style`用于设置文字的常规、斜体显示

- `normal`：常规显示
- `italic`：用字体的斜体显示
- `oblique`：文字倾斜显示

### font-variant

`font-variant`用于设置小写字母的显示形式

- `normal`：常规显示
- `small-caps`：将小写字母替换为缩小过的大写字母