# 网易云案例笔记

## topbar

### logo

网页中经常会用h1标签来放网站的logo，有利于SEO优化

logo可以用图片也可以用背景图片，精灵图一定用背景图片

```html
<h1 class="logo"><a hidefocus="true" href="/#">网易云音乐</a></h1>
```

可以用h1或a的背景设置logo

通过给 a 元素设置 `text-indent: -9999px;` 可以让文字消失

text-indent 属性对行内非替换元素无效

所以还需要给 a 元素设置 `display: block/inline-block;`

### line-height的继承

如果块级元素中没有内容，那么 line-height 属性继承过来也是不生效的

## banner

### 块级元素中嵌套图片

块级元素中的行内元素默认情况下是基线对齐的

所以块级元素中嵌套图片底部会多出来一些像素

解决方法

1. 设置图片的 vertical-align
2. 设置图片为块级元素

## main

### 块级元素中的居中对齐

加入块级元素中存在一个 inline-block 元素

块级元素的高度和行高相等

inline-block 元素的高度也和行高相等

由于 块级元素的 vertical-align 默认是 baseline

此时 inline-block 元素会在块级元素中垂直居中

但 inline-block 元素中字体大小大于块级元素时就会出现偏移

inline-block 元素的行高与高度不一致时也会出现偏移

```html
<div class="box">
	aaabcbauasffpp<span>aaaaxxxbbbppp</span>
</div>
```

```css
.box {
    height: 200px;
    line-height: 200px;
    background-color: orange;
    font-size: 14px;
}
.box span {
    display: inline-block;
    height: 60px;
    line-height: 60px;
    background-color: aqua;
    font-size: 14px;
}
```

### 申请按钮

实现方式是给外层和内层分别设置精灵图为背景

```html
<a href="#" class="btn-sprite btn-type-01-sup apply">
  <span class="btn-sprite btn-type-01-sub">申请成为网易音乐人</span>
</a>
```

```css
.btn-sprite {
  background: url(../images/button2.png) no-repeat 0 9999px;
  display: inline-block;
}

.btn-type-01-sup {
  background-position: right -100px;
  padding-right: 5px;
  border-radius: 4px;
  height: 31px;
  line-height: 31px;
  text-align: center;
  font-weight: bold;
  font-size: 12px;
}

.btn-type-01-sup:hover {
  background-position: right -182px;
}

.btn-type-01-sub {
  display: block;
  background-position: 0 -59px;
  height: 31px;
}

.btn-type-01-sup:hover .btn-type-01-sub {
  background-position: 0 -141px;
}
```

### 行内非替换元素中嵌套块级元素

规范是行内非替换元素中最好不要嵌套块级元素

但如果必须要这样做

需要给行内非替换元素设置 `display:inline-block;`

才可以设置宽、高、背景等属性

### display: inline-block的垂直空隙

设置 display: inline-block 可能会出现垂直空隙

将父元素的 font-size 设置为0 可以解决
