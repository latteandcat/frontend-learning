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

