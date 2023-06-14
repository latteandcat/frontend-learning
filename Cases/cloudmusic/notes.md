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

