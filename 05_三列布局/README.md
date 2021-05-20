# 【不一样的CSS】实现三列布局的 5 种方式

> **若彼岸繁华落尽，谁陪我看落日流年**


## 写在前面

对 CSS 布局掌握程度决定你在 Web 开发中的开发页面速度。随着 Web 技术的不断革新，实现各种布局的方式已经多得数不胜数了。

最近利用碎片时间，大概用了半个月的时间整理了一个系列，本系列文章总结了 CSS 中的各种布局，以及实现方式及其常用技巧。让你通过该系列文章对 CSS 布局有一个新的认识。

该系列的导航帖[点我进入](https://juejin.cn/post/6963251091035291656/)，里面可以快速跳转到你想了解的文章(建议收藏)

### 三列布局概述

三列布局主要分为两种：

- 第一种是前两列定宽，最后一列自适应，这一种本质上与两列布局没有什么区别，可以参照两列布局实现[]()

- 第二种是前后两列定宽，中间自适应，最终效果图如下

  ![](http://img.seecode.cc//picgo/%E3%80%90%E4%B8%8D%E4%B8%80%E6%A0%B7%E7%9A%84css%E3%80%91%E5%AE%9E%E7%8E%B0%E4%B8%89%E5%88%97%E5%B8%83%E5%B1%80.gif)

  该文章主要介绍第二种

## 实现三列布局的 5 种方式

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
.left {
  height: 400px;
  width: 200px;
  background-color: #f783ac;
}
.content {
  height: 400px;
  background-color: #d9480f;
}
.right {
  height: 400px;
  width: 200px;
  background-color: #c0eb75;
}
.left,
.content,
.right {
  font-size: 70px;
  line-height: 400px;
  text-align: center;
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
<!-- 解决高度塌陷 -->
<div class="container clearfix">
  <div class="left">左</div>
  <div class="content">内容</div>
  <div class="right">右</div>
</div>
```

### 通过 float 实现方案一

实现步骤：

1. 为了完成效果需要调整 HTML 结构，调整后如下：

   ```html
   <!-- 解决高度塌陷 -->
   <div class="container clearfix">
     <div class="left">左</div>
     <div class="right">右</div>
     <div class="content">内容</div>
   </div>
   ```

2. 左列容器开启左浮动

3. 右列容器开启右浮动

4. 自适应元素设置 overflow 会创建一个 BFC 完成自适应

完整 CSS 代码如下所示

```css
.left {
  /* 1. 左列容器开启左浮动 */
  float: left;
}
.content {
  /* 自适应元素设置 overflow 会创建一个BFC 完成自适应 */
  overflow: hidden;
}
.right {
  /* 2. 右列容器开启右浮动 */
  float: right;
}
```

### 通过 float 实现方案二

实现步骤：

1. 为了完成效果需要调整 HTML 结构，调整后如下：

   ```html
   <!-- 解决高度塌陷 -->
   <div class="container clearfix">
     <div class="left">左</div>
     <div class="right">右</div>
     <div class="content">内容</div>
   </div>
   ```

2. 左列容器开启左浮动

3. 右列容器开启右浮动

4. 使中间自适应的宽度为父级容器减去两个定宽的列

完整 CSS 代码如下所示

```css
.left {
  /* 1. 左列容器开启左浮动 */
  float: left;
}
.content {
  /* 3. 使中间自适应的宽度为父级容器减去两个定宽的列 */
  width: calc(100%-400px);
}
.right {
  /* 2. 右列容器开启右浮动 */
  float: right;
}
```

### 通过 position 实现

实现步骤

1. 左右两列脱离文档流，并通过偏移的方式到达自己的区域
2. 使中间自适应的宽度为父级容器减去两个定宽的列
3. 通过外边距将容器往内缩小

```css
.left {
  /* 1. 左右两列脱离文档流，并通过偏移的方式到达自己的区域 */
  position: absolute;
  left: 0;
  top: 0;
}
.content {
  /* 2. 使中间自适应的宽度为父级容器减去两个定宽的列 */
  width: calc(100%-400px);
  /* 3. 通过外边距将容器往内缩小 */
  margin-right: 200px;
  margin-left: 200px;
}
.right {
  position: absolute;
  right: 0;
  top: 0;
}
```

> 该方案实现的核心是 `calc()` 函数，关于 `calc()` 函数，可以通过我另一篇文章 [【不一样的CSS】一文让你了解CSS中的各种单位](https://juejin.cn/post/6964025107685572644) 鼠标滚动的最后进行了解。

### Flex 布局实现

通过 Flex 布局实现该功能主要是通过 `flex` 属性来实现示例代码如下：

```css
.container {
  display: flex;
}
.right {
  flex: 1;
  /* flex: 1; 表示 flex-grow: 1; 即该项占所有剩余空间 */
}
```

> 关于 Flex 布局的详细用法请参考 [点我进入](https://juejin.cn/post/6963250638214365220)

### Grid 布局实现

通过 Grid 布局实现该功能主要是通过 `template` 属性实现，具体代码如下所示：

```css
.container {
  display: grid;
  /* 将其划分为两行，其中一列有本身宽度决定， 一列占剩余宽度*/
  grid-template-columns: auto 1fr auto;
}
```

> 关于 Grid 布局的详细用法请参考 [点我进入](https://juejin.cn/post/6963252773202690055)

## 总结

![]()![实现三列布局的 5 种方式](http://img.seecode.cc//picgo/%E5%AE%9E%E7%8E%B0%E4%B8%89%E5%88%97%E5%B8%83%E5%B1%80%E7%9A%84%205%20%E7%A7%8D%E6%96%B9%E5%BC%8F.png)