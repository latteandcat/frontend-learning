# CSS

## 一、CSS编写样式的方式以及应用场景

CSS有三种常用的编写方式，分别是内联样式、内部样式表和外部样式表

* 内联样式的应用场景：在Vue的template中某些动态的样式会使用内联样式
* 内部样式表的应用场景：Vue开发中，每个组件都有一个style元素，使用的是内部样式表的方式，不过原理并不相同
* 外部样式表的应用场景：外部样式表是开发中最常用的方式，将所有CSS文件放在一个独立的文件夹中，然后通过link元素引入到需要的文件中，也可以在index.css文件中通过 @import url(路径) 引入其他CSS样式

## 二、最常见的CSS样式以及作用

最常见的CSS样式有：

* font-size：设置文字大小
* color：设置前景色(颜色)
* background-color：设置背景色
* width：设置宽度
* height：设置高度

## 三、CSS中颜色的表示方式

1. 颜色关键字：例如red，yellow等

2. RGB颜色

   所有颜色都是由三原色R(red)G(green)B(blue)组成，也就是通过调整这三个颜色不同的比例组合成其他的颜色，RGB各个原色的取值是0~255

   RGB的表示方式有以下三种

   1. 十六进制符号`#RRGGBB[AA]`
   2. 十六进制符号`#RGB[A]`是第一种形式的缩减版本
   3. 函数符`rgb[a](R, G, B[, A])`

3. HSL颜色

   被定义为色相 - 饱和度 - 亮度（Hue-saturation-lightness）模式
   
   表示方式为`hsl[a](H, S, L[, A])`

## 四、具体说明text-align居中的条件

text-align只能定义行内级元素相对它的块级父元素的对齐方式

对于块级元素，可以通过`display:inline-block;`来使其同时具备行内级元素和块级元素的特征

## 五、line-height为什么可以让文字居中

line-height 表示一行文字的高度，也就是两行文字基线之间的间距

当 line-height=height 时就可以使这行文字在div内部垂直居中

这是因为行距=行高-文本的高度，而行距是上下等分的，但只能对文本使用

## 六、CSS属性的两个特性

1. 继承性

   - 如果一个属性具备继承性，那么在该元素上设置后，它的后代元素都可以继承这个属性
   - 如果后代元素自己也设置了该属性，那么优先使用后代元素自己的属性（不管继承过来的属性权重多高）

2. 层叠性

   - 对于一个元素来说，一个相同的属性可以通过不同的选择器进行多次设置，此时属性会被一层层覆盖到元素上，但是最终只有一层会生效

   - 判断生效属性的方式

     - 根据选择器的权重判断：权重较大的选择器优先生效，根据选择器的权重判断出优先级

     - 根据先后顺序判断：选择器的权重相同时，后设置的生效

## 七、display常见的值及其作用

- block：让元素显示为块级元素

  - 独占一行
  - 可以设置宽高
  - 默认高度由内容决定

- inline：让元素显示为行内级元素

  - 非独占一行
  - 不可以设置宽高
  - 宽度和高度由内容决定

- inline-block：让元素同时具备行内级元素和块级元素的特征

  - 非独占一行
  - 可以设置宽高
  - 默认宽高由内容决定
- none：隐藏元素

## 八、元素隐藏的方法及其区别

1. display设置为none：元素不显示出来，并且也不占据任何空间
2. visibility设置为hidden：元素不可见，但是会占据元素应该占据的空间，设置为visible就会可见
3. 将rgba的a设置为0：将元素前景色或背景色变成透明的，不影响子元素
4. 将opacity设置为0：设置整个元素的透明度，会影响所有的子元素

## 九、写出盒子模型包含的内容以及如何设置

1. 内容：通过宽度和高度设置
2. 内边距：通过padding设置
3. 边框：通过border设置
4. 外边距：通过margin设置

## 十、margin的传递和折叠

margin的上下传递

- 如果块级元素的顶部线和父元素的顶部线重叠，那么这个块级元素的**margin-top**会传递给父元素
- 如果块级元素的底部线和父元素的底部线重叠，并且**父元素的高度是auto**，那么这个块级元素的**margin-bottom**会传递给父元素

margin的上下折叠

- 垂直方向上相邻的2个margin（margin-top、margin-bottom）有可能会合并为1个margin
- 折叠可能出现在两个兄弟块级元素之间，也可能出现在父子块级元素之间

## 十一、box-sizing的作用及其取值的区别

`box-sizing`用来设置盒子模型中宽高的行为

- `content-box`：padding和border都布置在width/height以外

  元素的实际占用宽度=border+padding+width

  元素的实际占用高度=border+padding+height

- `border-box`：padding和border都布置在width/height以内

  元素的实际占用宽度=width

  元素的实际占用高度=height


## 十二、说出元素水平居中的方案以及对应的场景

* 行内块元素(包括inline-block元素)

  * 水平居中：在父元素中设置text-align: center

* 块级元素 

  * 水平居中:margin:0 auto;

## 十三、结构伪类的nth-child和nth-of-type的区别

:nth-child 用于选择元素的子元素的一部分，不会考虑元素的类型

:nth-of-type 用于选择元素的同种类型子元素的一部分

## 十四、总结浮动的规则

1. 元素浮动后会脱离标准流

   - 朝着向左或向右方向移动，直到自己的边界紧贴着**包含块**（一般是父元素）或者其他浮动元素的边界为止

   - 定位元素会层叠在浮动元素上面

2. 如果元素是向左（右）浮动，浮动元素的左（右）边界不能超出包含块的左（右）边界

3. 浮动元素之间不能层叠

   - 如果一个元素浮动，另一个浮动元素已经在那个位置了，后浮动的元素将紧贴着前一个浮动元素
   - 如果水平方向剩余的空间不够显示浮动元素，浮动元素将向下移动，直到有充足的空间为止

4. 浮动元素不能与行内级内容层叠，行内级内容将会被浮动元素推出

   比如：行内级元素、inline-block元素、块级元素的文字内容

5. 行内级元素、inline-block元素浮动后，其顶部将与所在行的顶部对齐

## 十五、为什么需要清除浮动以及如何清除浮动

由于浮动元素会脱离标准流，父元素计算总高度的时候，不会计算浮动子元素的高度，这会导致父元素的**高度塌陷**问题

解决父元素高度塌陷问题的过程，一般叫做**清除浮动**

清除浮动的目的是让父元素计算总高度的时候，把浮动子元素的高度算进去

清除浮动的方法：

1. 给父元素设置固定高度

   缺点：扩展性不好

2. 在父元素最后增加一个空的块级子元素，并且设置`clear`属性为both

   缺点：会增加很多无意义的空标签，违反结构与样式分离的原则

3. 给父元素添加一个伪元素

```css
.clear-fix::after {
  content: "";
  display: block;
  clear: both;
  visibility: hidden; /* 浏览器兼容性 */
  height: 0; /* 浏览器兼容性 */
}
.clear-fix {
  *zoom: 1; /* IE6/7兼容性 */
}
```

## 十六、总结flex布局container和item的属性以及作用

1. flex container 的相关属性
   - `flex-direction`：用于设置`main axis`的方向
   - `flex-wrap`：决定`flex container`是否多行
   - `flex-flow`：`flex-direction` 和 `flex-wrap` 的简写
   - `justify-content`：决定`flex items`在`main axis`上的对齐方式
   - `align-items`：决定`flex items`在`cross axis`上的对齐方式
   - `align-content`：决定多行`flex items`在`cross axis`上的对齐方式
2. flex item 的相关属性
   - `order`：决定 flex items 的排布顺序
   - `align-self`：用于覆盖 flex container 设置的 `align-items`
   - `flex-grow`：决定 flex items 如何扩展
   - `flex-shrink`：决定 flex items 如何收缩
   - `flex-basis`：用于设置 flex items 在主轴方向上的 base size
   - `flex`：简写属性

## 十七、说出常见的CSS Transform形变有哪些

 transform属性允许对某一个元素进行某些形变，包括旋转、缩放、倾斜或平移等。transform对于行内级非替换元素（如a，span）是无效的。

* translate(x, y)：平移，用于移动元素在平面上的位置 
* scale(x, y) ：缩放，可改变元素的大小
* rotate(deg) ：旋转，表示旋转的角度 
* skew(deg, deg) ：倾斜，定义了一个元素在二维平面上的倾斜转换 

## 十八、说出CSS Transition和Animation动画的区别

- transition

  * 只能定义两个状态：开始状态和结束状态，不能定义中间状态

  * 不能重复执行动画，除非一再触发动画

  * 需要在特定状态触发后才能执行，比如某属性修改了

- animation

  * 可以用@keyframes定义动画序列（每一帧如何执行）

  * 通过设置animation-iteration-count来规定动画执行的次数

  * 不需要触发特定状态即可执行

- animation动画比transition多了animation-iteration-count， animation-direction， animation-fill-mode 和 animation-play-state属性


## 十九、说出不同像素之间的差异

- 设备像素，即物理像素
  - 设备像素指的是显示器上的真实像素
  - 设备像素为显示器屏幕的固有属性，不会改变
  - 设备分辨率对应的是设备像素
- 设备独立像素，也叫做逻辑像素
  - 逻辑像素指的是操作系统抽象的像素
  - 显示分辨率对应的是逻辑像素
- CSS 像素
  - CSS中使用的像素在默认情况下等同于逻辑像素

## 二十、说出你对视口的理解

- pc端视口就是浏览器的可视区域

- 移动端视口

  - 布局视口：一个默认宽度为980px的用于布局页面内容的视口

    ```html
    <!-- width: 设置布局视口的宽度 -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    ```

  - 视觉视口：布局视口会缩小到视觉视口并显示在可见区域

  - 理想视口：当布局视口 = 视觉视口的时候，就是理想视口

    ```html
    <!-- 设置理想视口 -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no, minimum-scale=1.0, maximum-scale=1.0">
    ```
