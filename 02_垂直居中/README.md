# 【不一样的CSS】实现垂直布局的 8 种方式

> **若彼岸繁华落尽，谁陪我看落日流年**


## 写在前面

对 CSS 布局掌握程度决定你在 Web 开发中的开发页面速度。随着 Web 技术的不断革新，实现各种布局的方式已经多得数不胜数了。

最近利用碎片时间，大概用了半个月的时间整理了一个系列，本系列文章总结了 CSS 中的各种布局，以及实现方式及其常用技巧。让你通过该系列文章对 CSS 布局有一个新的认识。

该系列的导航帖[点我进入](https://juejin.cn/post/6963251091035291656/)，里面可以快速跳转到你想了解的文章(建议收藏)

## 实现垂直布局的 8 种方式

### 1. 单行文本垂直居中

若文本为单行文本的话，直接使用 `line-height` 等于父元素的高度即可实现。示例代码如下：

HTML 代码

```html
<div class="text">这是一个需要居中的测试文本</div>
```

CSS 代码

```css
.text {
  height: 200px;
  font-size: 3rem;
  font-weight: bold;
  background-color: #ff8787;
  text-align: center;
  /* 通过 line-height 等于元素高度即可完成文字垂直居中 */
  line-height: 200px;
}
```

执行结果如下：

![image-20210518204003928](http://img.seecode.cc//picgo/image-20210518204003928.png)

### 2. 行内块级元素垂直居中

若元素是行内块级元素, 基本思想是子元素使用 `display: inline-block, vertical-align: middle` 并让父元素行高等同于高度。示例代码如下所示：

HTML 代码

```html
<div class="parent">
  <div class="child"></div>
</div>
```

CSS 代码

```css
.parent {
  height: 500px;
  width: 300px;
  margin: 0 auto;
  background-color: #ff8787;
  /* 为父级容器设置行高 */
  line-height: 500px;
}
.child {
  width: 300px;
  height: 300px;
  /* 将子级元素设置为 inline-block 元素 */
  display: inline-block;
  /* 通过 vertical-align: middle; 实现居中 */
  vertical-align: middle;
  background-color: #91a7ff;
}
```

执行结果如下

![image-20210518211344550](http://img.seecode.cc//picgo/image-20210518211344550.png)

### 3. 使用 position + top + height + -margin 实现垂直居中

> 关于定位的知识，可以从该专题的导航帖[点我进入](https://juejin.cn/post/6963251091035291656/)去看一下[深入理解position](https://juejin.cn/post/6963255405292355620)



`top: 50%; margin-top: 等于负的高度的一半` 就可以实现垂直居中，示例代码如下：

HTML 代码

```html
<div class="parent">
  <div class="child"></div>
</div>
```

CSS 代码

```css
.parent {
  height: 500px;
  width: 300px;
  margin: 0 auto;
  background-color: #ff8787;
  /* 为父级容器开启相对定位 */
  position: relative;
}
.child {
  width: 300px;
  height: 300px;
  background-color: #91a7ff;
  position: absolute;
  top: 50%;
  /* margin-top: 等于负高度的一半 */
  margin-top: -150px;
}
```

执行结果同上

### 4. 使用 position + top + bottom + height + margin 实现垂直居中

`top` 和 `bottom` 将子元素拉伸至 `100%`，设置指定的高度，通过 `margin:auto;` 即可实现垂直居中，示例代码如下：

HTML 代码

```html
<div class="parent">
  <div class="child"></div>
</div>
```

CSS 代码

```css
.parent {
  height: 500px;
  width: 300px;
  margin: 0 auto;
  background-color: #ff8787;
  /* 为父级容器开启相对定位 */
  position: relative;
}
.child {
  width: 300px;
  height: 300px;
  background-color: #91a7ff;
  position: absolute;
  /* 垂直拉满 */
  top: 0;
  bottom: 0;
  /* margin: auto 即可实现 */
  margin: auto;
}
```

执行结果同上

### 5. 使用 position + top + transform 实现垂直居中

通过 `top:50%;` 和 `translateY(-50%)` 即可实现。

HTML 代码

```html
<div class="parent">
  <div class="child"></div>
</div>
```

CSS 代码

```css
.parent {
  height: 500px;
  width: 300px;
  margin: 0 auto;
  background-color: #ff8787;
  /* 为父级容器开启相对定位 */
  position: relative;
}
.child {
  width: 300px;
  height: 300px;
  background-color: #91a7ff;
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
}
```

执行结果同上

### 6. 使用 border-box + padding 实现垂直居中

使用这种方式存在局限性，`padding-top/bottom` 各占剩余高度的一半即可实现，这里不做代码展示了，开发实际中用的很少。

### 7. 使用 Flex 实现垂直居中

> 关于 Flex 布局的详细用法请参考 [点我进入](https://juejin.cn/post/6963250638214365220)

使用 Flex 实现一个垂直居中非常的容器，具体代码如下：

CSS 代码

```css
.parent {
  height: 500px;
  width: 300px;
  margin: 0 auto;
  background-color: #ff8787;
  /* 开启 flex 布局 */
  display: flex;
  /* 方法一 */
  /* align-items: center; */
}
.child {
  /* 方法二 */
  margin: auto;
  width: 300px;
  height: 300px;
  background-color: #91a7ff;
}
```

HTML 代码和效果图同上

### 8. 使用 Grid 实现垂直居中

> 关于 Grid 布局的详细用法请参考 [点我进入](https://juejin.cn/post/6963252773202690055)

开启 Grid 布局也也可以实现居中效果，不过仅仅实现一个居中就有点大材小用了。示例代码如下：

CSS 代码

```css
.parent {
  height: 500px;
  width: 300px;
  margin: 0 auto;
  background-color: #ff8787;
  display: grid;
  /* 方法一 */
  /* align-items: center; */
  /* 方法二 */
  /* align-content: center; */
}
.child {
  /* 方法三 */
  /* margin: auto; */
  /* 方法四 */
  align-self: center;
  width: 300px;
  height: 300px;
  background-color: #91a7ff;
}
```

HTML 代码和效果图同上

> 值得注意的是应该在网格容日上的样式仅仅对当前网格容日有效。

## 总结

![](http://img.seecode.cc//picgo/%E5%AE%9E%E7%8E%B0%E5%9E%82%E7%9B%B4%E5%B8%83%E5%B1%80%E7%9A%84%208%20%E7%A7%8D%E6%96%B9%E5%BC%8F.png)