# 【不一样的CSS】实现全屏布局的 3 种方式

> **若彼岸繁华落尽，谁陪我看落日流年**


## 写在前面

对 CSS 布局掌握程度决定你在 Web 开发中的开发页面速度。随着 Web 技术的不断革新，实现各种布局的方式已经多得数不胜数了。

最近利用碎片时间，大概用了半个月的时间整理了一个系列，本系列文章总结了 CSS 中的各种布局，以及实现方式及其常用技巧。让你通过该系列文章对 CSS 布局有一个新的认识。

该系列的导航帖[点我进入](https://juejin.cn/post/6963251091035291656/)，里面可以快速跳转到你想了解的文章(建议收藏)

### 全屏布局概述

全部布局主要应用在后台，主要效果如下所示：

![image-20210521214013935](http://img.seecode.cc//picgo/image-20210521214013935.png)

## 实现全屏布局的 3 种方式


在开始今天的文章之前，我们先把今天的主要代码放到下面：

公共的 CSS 样式如下：

```css
body {
  margin: 0;
}
body,
html,
.container {
  height: 100vh;
  box-sizing: border-box;
  text-align: center;
  overflow: hidden;
}
.content {
  background-color: #52c41a;
  /* * 中间部门的布局可以参考 两列 三列 布局 */
  display: grid;
  grid-template-columns: auto 1fr;
}
.left {
  width: 240px;
  background-color: #52c41a;
  font-size: 80px;
  line-height: calc(100vh - 200px);
}
.right {
  background-color: #f759ab;
  font-size: 60px;
}
.header {
  height: 100px;
  background-color: #70a1ff;
}
.footer {
  height: 100px;
  background-color: #ff7a45;
}
.header,
.footer {
  line-height: 100px;
  font-size: 52px;
}
```

HTML 结构如下：

```html
<div class="container">
    <div class="header">header</div>
    <div class="content">
        <div class="left">导航</div>
        <div class="right">
            <div class="right-in">自适应，超出高度出现滚动条</div>
        </div>
    </div>
    <div class="footer">footer</div>
</div>
```

关于中间的三列布局可以参考 []()

### 使用calc函数实现

实现步骤如下：

1. 通过 calc 函数计算出中间容器的高度。
2. 中间出现滚动条的容器设置 `overflow: auto` 即出现滚动条的时候出现滚动条。

实现代码如下：

```css
.content {
    overflow: hidden;
    /* 通过 calc 计算容器的高度 */
    height: calc(100vh - 200px);
}
.left {
    height: 100%;
}
.right {
    /* 如果超出出现滚动条 */
    overflow: auto;
    height: 100%;
}
.right-in {
    /* 假设容器内有500px的元素 */
    height: 500px;
}
```

### 使用 flex 方案

使用 Flex 方式实现该布局比较简单，示例代码如下所示：

```css
.container {
    /* 开启flex布局 */
    display: flex;
    /* 将子元素布局方向修改为垂直排列 */
    flex-flow: column;
}
.content {
    /* 如果超出出现滚动条 */
    overflow: auto;
    /* 设置 中间 部分自适应 */
    flex: 1;
}
.right-in {
    /* 假设容器内有500px的元素 */
    height: 500px;
}
```

> 关于 Flex 布局的详细用法请参考 [点我进入](https://juejin.cn/post/6963250638214365220)

### 使用 grid 方案

grid 布局对于这种布局来说，实现起来是非常得心应手的，通过 `template` 属性即可实现，示例代码如下：

```css
.container {
    /* 开启grid布局 */
    display: grid;
    grid-template-rows: auto 1fr auto;
}
.content {
    /* 如果超出出现滚动条 */
    overflow: auto;
}
.right-in {
    /* 假设容器内有500px的元素 */
    height: 500px;
}
```

> 关于 Grid 布局的详细用法请参考 [点我进入](https://juejin.cn/post/6963252773202690055)

## 总结

![](http://img.seecode.cc//picgo/%E5%AE%9E%E7%8E%B0%E5%85%A8%E5%B1%8F%E5%B8%83%E5%B1%80%E7%9A%84%203%20%E7%A7%8D%E6%96%B9%E5%BC%8F.png)