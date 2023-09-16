# CSS 选择器

## 什么是 CSS 选择器

CSS 选择器用于按照一定的规则选出符合条件的元素，为之添加 CSS 样式

## 选择器的种类

### 通用选择器

通过 `*` 选择所有元素，一般用来给所有元素做一些通用性的设置

效率比较低，不推荐使用

### 元素选择器⭐

通过元素的名称选择同一种元素

### 类选择器⭐

通过 `.classname` 选择使用同一类名的元素

### id选择器⭐

通过 `#id` 选择元素

> 一个 HTML 文档里面的 id 值是唯一的，不能重复
>
> id 如果由多个单词组成，单词之间可以用中划线、下划线或驼峰标识
>
> 最好不要用标签名作为 id 值

### 属性选择器

通过元素的属性选择元素

- 拥有某一个属性： `[attr]`

- 属性等于某一个值： `[attr=val]`

- 属性值包含 val： `[attr*=val]`

- 属性值以 val 开头： `[attr^=val]`

- 属性值以 val 结尾： `[attr$=val]`

- 属性值等于 val，或者以 val 开头后面紧跟连接符 `-`：`[attr|=val]`

  val, val-xxx

- 属性值包含 val，如果有其他值必须以空格和 val 分割：`[attr~=val]`

  val, val xxx

### 后代选择器⭐

1. 选择所有后代 `父元素 子元素`

   选择器之间以空格分割

2. 选择直接子代 `父元素 > 子元素`

   选择器之间以 `>` 分割

### 兄弟选择器

1. 相邻兄弟选择器

   选择器之间以 `+` 分割

2. 普遍兄弟选择器

   选择器之间以 `~` 分割

### 选择器组合⭐

1. 交集选择器

   需要同时符合两个选择器条件，两个选择器紧密连接

   在开发中通常是为了精准的选择某一个元素

   eg: `div.one`

2. 并集选择器

   符合一个选择器条件即可，两个选择器以 `,` 分割

   在开发中通常是为了给多个元素设置相同的样式

   eg: `.one, .two`

### 伪类

[伪类（Pseudo-classes）](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-classes)是选择器的一种，用于选择处于**特定状态**的元素

- 伪类由冒号后面跟着伪类名称组成

- 函数式伪类还包含一对括号来定义参数

- 附上伪类的元素叫做锚元素

**常用的伪类**

1. 用户行为伪类：与用户的交互行为有关

   - `:hover`：用户鼠标悬停
   - `:active`：被用户激活
   - `:focus`：获得焦点
   
2. 定位伪类：与链接（a元素）和 URL 定位的当前文档中的元素有关

   - `:link`：未访问过的链接
   - `:visited`：访问过的链接
   - `:any-link`：所有链接
   - `:local-link`：本页面内跳转的链接
   - `:target`：URL 中定位的元素

   > **a 元素伪类的注意事项**
   >
   > a:hover 必须放在 a:link 和 a:visited 后面才能完全生效
   >
   > a:active 必须放在 a:hover 后面才能完全生效
   >
   > :focus 也可以用于 a 元素
   >
   > 所以建议的编写顺序是 **a:link、a:visited、a:focus、a:hover、a:active**
   >
   > 此外，直接给 a 元素设置样式，相当于给 a 元素的所有动态伪类都设置了样式

3. 输入伪类：与表单元素有关

   - `:autofill`
   - `:enabled`
   - `:disabled`
   - `:checked`
   - `:default`
   - `:blank`
   - `:read-only`
   - `:required`
   
4. 树结构伪类
   - `:root`：根元素即 html 元素
   
   - `:empty`：内容为空的元素
   
   - `:nth-child 和 :nth-last-child`
   
     ```css
     :nth-child(1) /* 第一个子元素 */
     :nth-child(2n) /* 第偶数个子元素 */
     :nth-child(2n+1) /* 第奇数个子元素 */
     :nth-child(-n+2) /* 前两个子元素 */
     
     :nth-last-child(1) /* 最后一个子元素 */
     :nth-last-child(-n+2) /* 最后两个子元素 */
     ```
   
   - `:first-child 和 :last-child`
   
   - `:only-child`：父元素中唯一子元素
   
   - `:nth-of-type 和 :nth-last-of-type`
   
      ```css
      .box > div:nth-of-type(3) /* 选择box中的第三个div */
      .box > div:nth-last-of-type(3) /* 选择box中的倒数第三个div */
      ```
   
   - `:first-of-type :last-of-type`
   
   - `:only-of-type`：父元素中唯一这种类型的元素
   
5. 语言伪类

   - `:lang()`：基于其内容语言来选择元素
   
   
      - `:dir()`：基于文档语言的方向性（ rtl / ltr）来选择元素
   
6. 元素显示状态伪类

   - `:fullscreen`：匹配当前处于全屏模式的元素
   
   
      - `:picture-in-picture`：匹配当前处于画中画模式的元素
   

### 伪元素

伪元素可以是一个冒号也可以是两个冒号

为了区分伪元素和伪类，建议使用两个冒号

**常用的伪元素**

- `::first-line`：针对首行文本设置属性

- `::first-letter`：可以针对首字母设置属性

- `::before`和`::after`⭐

  主要用来在一个元素的内容之前或之后插入其他内容（可以是文字、图片)

  常通过 content 属性来为一个元素添加修饰性的内容

  ```css
  .box::before {
    content: "123";
    color: red;
  }
  .box::after {
    content: "321";
    color: blue;
  }
  ```

  常用于在元素后面或前面添加图标

  ```css
  .item::after {
    content: url("icon.svg");
    color: green;
    font-size: 20px;
    /* 调整图标位置 */
    position: relative;
    left: 5px;
    top: 2px;
  }
  ```

  注意：在使用伪元素的过程中，**不要将content省略**

  ```css
  .box::after {
    content: "";
    display: inline-block;
    width: 8px;
    height: 8px;
    background-color: #f00;
  }
  ```
  
  将 content 设置为图片时伪元素的 width 和 height 无法控制图片大小

  解决办法是在伪元素的 background 中设置图片，通过 `background-size: cover;` 和宽高来控制图片大小
  
  ```css
  .item::before {
    content: '';
    display: block;
    width: 14px;
    height: 14px;
    background:url('img.png')
    background-size:cover;
  }
  ```

## 选择器的权重

为了比较 CSS 层叠属性的优先级，可以给 CSS 属性所处的选择器定义一个权重

- !important：10000
- 内联样式：1000
- id 选择器：100
- 类选择器、属性选择器、伪类：10
- 元素选择器、伪元素：1
- 通配符：0
