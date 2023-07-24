# CSS预处理器

## CSS编写的痛点

- CSS 作为一种样式语言，本身用来给HTML元素添加样式是没有问题的

- 但是目前前端项目已经越来越复杂，不再是简简单单的几行CSS就可以搞定的，我们需要成千上万行的CSS来完成页面的美化工作

- 随着代码量的增加，必然会造成很多的编写不便
  - 比如大量的重复代码，虽然可以用类来勉强管理和抽取，但是使用起来依然不方便
  - 比如无法定义变量（当然目前已经支持），如果一个值被修改，那么需要修改大量代码，可维护性很差（比如主题颜色）
  -  比如没有专门的作用域和嵌套，需要定义大量的id/class来保证选择器的准确性，避免样式混淆
  - 等等一系列的问题

- 为了解决CSS面临的大量问题，出现了一系列的CSS预处理器（CSS preprocessor）
  - CSS 预处理器是一个能让你通过预处理器自己独有的语法来生成CSS的程序
  - 市面上有很多CSS预处理器可供选择，且绝大多数CSS预处理器会增加一些原生CSS不具备的特性
  - 代码最终会转化为CSS来运行, 因为对于浏览器来说只识别CSS

## 常见的 CSS 预处理器

- Sass/Scss
  - 2007年诞生，最早也是最成熟的CSS预处理器，拥有ruby社区的支持，是属于Haml（一种模板系统）的一部分
  -  目前受LESS影响，已经进化到了全面兼容CSS的SCSS
- Less
  - 2009年出现，受SASS的影响较大，但又使用CSS的语法，让大部分开发者更容易上手
  - 比起SASS来，可编程功能不够，不过优点是使用方式简单、便捷，兼容CSS，并且已经足够使用
  - 另外反过来也影响了SASS演变到了SCSS的时代
  - 著名的 Twitter Bootstrap 就是采用LESS做底层语言的，也包括 React 的 UI 框架 AntDesign
- Stylus
  - 2010年产生，来自Node.js社区，主要用来给Node项目进行CSS预处理支持
  - 语法偏向于Python, 使用率相对于Sass/Less少很多

## Less

### 认识 Less

- Less 的官方介绍：It's CSS, with just a little more.

- Less 即 Leaner Style Sheets，是一门CSS扩展语言，并且兼容CSS

- Less 增加了很多更好用的特性

  比如：定义变量、混入、嵌套、计算等等

- Less 最终需要被编译成CSS运行于浏览器中（包括部署到服务器中）

### Less 的基本使用

1. 编写 Less 代码

2. 将 Less 代码编译成 CSS 代码

   编译的方式

   1. npm 下载 less 工具对代码进行编译

      ```bash
      npm install -g less
      lessc styles.less styles.css
      ```

   2. 通过 VS Code 插件编译

   3. [在线编译](https://lesscss.org/less-preview/)

   4. 引入 CDN 的 Less 编译代码，对 Less 进行实时的处理

      ```html
      <link rel="stylesheet/less" type="text/css" href="styles.less" />
      <script src="https://cdn.jsdelivr.net/npm/less"></script>
      ```

      也可以将 less 编译代码下载到本地，执行 js 代码对 less 进行编译

### Less 的常见语法

1. Less 是兼容 CSS 的

   我们可以在 Less 文件中编写所有的 CSS 代码

   只是文件扩展名不同

2. Less 中可以定义变量（Variables）

   在一个大型的网页项目中，CSS 中使用的某几种属性值往往是特定的

   将常见的颜色或者字体等定义为变量来使用

   定义变量的方式

   ```less
   @themeColor: #f3c258;
   
   .box {
       color: @themeColor;
   }
   ```

3. Less 支持选择器的嵌套（Nesting）

   当我们需要找到一个内层的元素时，往往需要嵌套很多层的选择器

   Less 提供了选择器的嵌套

   符号&表示当前选择器的父级

   ```less
   p {
       a.link {
           color: @mainColor;
           &:hover {
               color: @hoverColor;
           }
       }
   }
   ```

4. 运算（Operations）

   在 Less 中，算术运算符 `+、-、* 、/` 可以对任何数字、颜色或变量进行运算

   - 算术运算符在加、减或比较之前会进行单位换算，计算的结果以最左侧操作数的单位类型为准

   - 如果单位换算无效或失去意义，则忽略单位

     比如 100px + 10%

     或者颜色的相加

5. 混入（Mixins）

   在原来 CSS 的编写过程中，多个选择器中可能会有大量相同的代码

   我们希望可以将这些代码抽取到一个独立的地方，任何选择器都可以进行复用

   在less中提供了混入（Mixins）来帮助我们完成这样的操作

   **混入（Mixin）**是一种将一组属性从一个规则集混入到另一个规则集的方法

   ```less
   .border() {
       border-top: 2px solid #f00;
       border-bottom: 2px solid #0f0;
   }
   
   .container {
       height: 200px;
       .border()；
       
       .box {
       	height: 100px;
       	.border()；
       }
   }
   ```

   Note：混入在没有参数的情况下，小括号可以省略，但是不建议这样使用

   混入也可以**传入参数**（定义变量）
    ```less
   .border(@borderWidth: 5px, @borderColor: purple) {
       border: @borderWidth solid @borderColor;
   }
   
   .container {
       height: 200px;
       .border(10px, orange)；
       
       .box {
       	height: 100px;
       	.border()；
       }
   }
    ```
   
   混入**和映射结合**使用
   
   作用：可以弥补 less 中不能自定义函数的缺陷                                                  
   
   ```less
   .box_size() {
       width: 100px;
       height: 100px;
   }
   
   .box {
       width: .box_size()[width];
   }
   ```

6. 映射（Maps）

   ```less
   .colors() {
       primaryColor: #f00;
       secondColor: #0f0;
   }
   
   .box {
       width: 100px;
       height: 100px;
       color: .colors()[primaryColor];
       background-color: .colors()[secondColor];
   }
   ```

   和混入结合当作自定义函数使用

   ```less
   .pxToRem(@px) {
       result: (@px / @htmlFontSize) * 1rem
   }
   
   .box {
   	width: .pxToRem(100)[result];
   	height: .pxToRem(18)[result];
   }
   ```

7. 继承（extends）

   和混入作用类似，也用于复用代码

   但继承的代码最终会转化为并集选择器

   ```less
   .box_border {
   	border: 5px solid #f00;
   }
   
   .box {
       width: 100px;
       background-color: orange;
       &:extend(.box_border);
   }
   ```

8. 内置函数

   Less 内置了多种函数用于转换颜色、处理字符串、算术运算等

   [内置函数手册](https://less.bootcss.com/functions/)

9. 作用域（Scope）

   在查找一个变量时，首先在本地查找变量和混入（mixins）

   如果找不到，则从“父”级作用域继承

10. 注释（Comments）

    在Less中，块注释和行注释都可以使用

    ```less
    // 单行注释
    /*
    多行注释
    */
    ```

11. 导入（Importing）

    - 导入的方式和CSS的用法是一致的

    - 导入一个 `.less` 文件，此文件中的所有变量就可以全部使用了
    - 如果导入的文件是 `.less` 扩展名，则可以将扩展名省略掉

## Sass 和 Scss

### 认识 Sass 和 Scss

最初 Sass 是 Haml 的一部分，Haml 是一种模板系统，由 Ruby 开发者设计和开发

所以，Sass 的语法使用的是类似于 Ruby 的语法，没有花括号，没有分号，具有严格的缩进

它的语法和 CSS 区别很大，后来官方推出了全新的语法 Scss，意思是 Sassy CSS，它是完全兼容 CSS 的

- Scss 的语法也包括变量、嵌套、混入、函数、操作符、作用域等
- 通常也包括更为强大的控制语句、更灵活的函数、插值语法等
- [Scss指南](https://sass-lang.com/guide/)

