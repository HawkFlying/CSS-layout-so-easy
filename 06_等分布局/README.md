# 【不一样的CSS】实现等分布局的 4 种方式

> **若彼岸繁华落尽，谁陪我看落日流年**


## 写在前面

对 CSS 布局掌握程度决定你在 Web 开发中的开发页面速度。随着 Web 技术的不断革新，实现各种布局的方式已经多得数不胜数了。

最近利用碎片时间，大概用了半个月的时间整理了一个系列，本系列文章总结了 CSS 中的各种布局，以及实现方式及其常用技巧。让你通过该系列文章对 CSS 布局有一个新的认识。

该系列的导航帖[点我进入](https://juejin.cn/post/6963251091035291656/)，里面可以快速跳转到你想了解的文章(建议收藏)

### 等分布局概述

等分布局就是将一个容器平均分成几等份，这里以 4 等分为例，主要介绍4种方法。

## 实现等分布局的 4 种方式

在开始今天的文章之前，我们先把今天的主要代码放到下面：

公共的 CSS 样式如下：

```css
body {
  margin: 0;
}
.container {
  height: 400px;
  background-color: #eebefa;
}
.item {
  height: 100%;
}
.item1 {
  background-color: #eccc68;
}
.item2 {
  background-color: #a6c1fa;
}
.item3 {
  background-color: #fa7d90;
}
.item4 {
  background-color: #b0ff70;
}
/* 清除浮动 */
.clearfix:after {
  content: '';
  display: block;
  height: 0;
  clear: both;
  visibility: hidden;
}
```

HTML 结构如下：

```html
<!-- 父元素清除浮动 -->
<div class="container clearfix">
  <div class="item item1"></div>
  <div class="item item2"></div>
  <div class="item item3"></div>
  <div class="item item4"></div>
</div>
```

最终的效果如下图：

![image-20210520230721489](http://img.seecode.cc//picgo/image-20210520230721489.png)

### 浮动+百分比实现等分布局

这种方式比较简单，开启浮动，使每个元素占 `25%` 的宽度。CSS 代码如下：

```css
.item {
  /* 开启浮动，每个元素占 25% 的宽度 */
  width: 25%;
  float: left;
}
```

### 行内块级+百分比实现等分布局

这种方式与上面那种方式类似，不过需要注意的是行内块级元素有一些类似于边距的几个像素，导致各占25会超出容器。CSS代码如下：

```css
.item {
  /* 设置每个元素为行内块级元素，每个元素占 24.5% 的宽度 */
  width: 24.5%;
  /* 因为行内块级元素有一些类似于边距的几个像素，导致各占25会超出容器 */
  display: inline-block;
}
```

### Flex 布局实现

通过 Flex 布局实现该功能主要是通过 `flex` 属性来实现示例代码如下：

```css
.container {
  /* 开启 flex 布局 */
  display: flex;
}
.item {
  /* 每个元素占相同的宽度 */
  flex: 1;
}
```

> 关于 Flex 布局的详细用法请参考 [点我进入](https://juejin.cn/post/6963250638214365220)

### Grid 布局实现

通过 Grid 布局实现该功能主要是通过 `template` 属性实现，具体代码如下所示：

```css
.container {
  /* 开启 grid 布局 */
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  /* 使用 repeat 函数生成如下代码 */
  /* grid-template-columns: 1fr 1fr 1fr 1fr; */
}
```

> 关于 Grid 布局的详细用法请参考 [点我进入](https://juejin.cn/post/6963252773202690055)

## 总结

![](http://img.seecode.cc//picgo/%E5%AE%9E%E7%8E%B0%E7%AD%89%E5%88%86%E5%B8%83%E5%B1%80%E7%9A%84%204%20%E7%A7%8D%E6%96%B9%E5%BC%8F.png)