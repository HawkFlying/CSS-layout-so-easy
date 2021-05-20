# 【不一样的CSS】实现水平垂直布局的 7 种方式

> **若彼岸繁华落尽，谁陪我看落日流年**


## 写在前面

对 CSS 布局掌握程度决定你在 Web 开发中的开发页面速度。随着 Web 技术的不断革新，实现各种布局的方式已经多得数不胜数了。

最近利用碎片时间，大概用了半个月的时间整理了一个系列，本系列文章总结了 CSS 中的各种布局，以及实现方式及其常用技巧。让你通过该系列文章对 CSS 布局有一个新的认识。

该系列的导航帖[点我进入](https://juejin.cn/post/6963251091035291656/)，里面可以快速跳转到你想了解的文章(建议收藏)

## 实现水平垂直布局的 7 种方式

在开始今天的文章之前，我们先把今天的主要代码放到下面：

公共的 CSS 样式如下：

```css
body {
  margin: 0;
}
.parent {
  height: 500px;
  width: 500px;
  background-color: #eebefa;
  margin: 0 auto;
}
.child {
  height: 300px;
  width: 300px;
  background-color: #f783ac;
}
```

HTML 结构如下：

```html
<div class="parent">
  <div class="child"></div>
</div>
```

最终的效果如下图：

![image-20210520215354624](http://img.seecode.cc//picgo/image-20210520215354624.png)

### 1. 行内块级水平垂直居中方案

步骤如下：

1. 容器元素行高等于容器高度
2. 通过 `text-align: center;` 实现水平居中
3. 将子级元素设置为水平块级元素
4. 通过 `vertical-align: middle;` 实现垂直居中

完整CSS代码如下：

```css
.parent {
  /* 1. 设置行高等于容器高度 */
  line-height: 500px;
  /* 通过 text-align: center; 实现水平居中 */
  text-align: center;
}
.child {
  /* 将子级元素设置为水平块级元素 */
  display: inline-block;
  /* 通过 vertical-align: middle; 实现垂直居中 */
  vertical-align: middle;
}
```

### 定位+定宽+定高实现水平垂直居中方案(一)

步骤如下：

1. 使子元素相对于容器元素定位
2. 子元素开启绝对定位
3. 设置该元素的偏移量，值为 50% 减去宽度/高度的一半

完成CSS代码如下：

```css
.parent {
  /* 1. 使子元素相对于本元素定位 */
  position: relative;
}
.child {
  /* 2. 开启绝对定位 */
  position: absolute;
  /* 3. 设置该元素的偏移量，值为 50%减去宽度/高度的一半 */
  left: calc(50% - 150px);
  top: calc(50% - 150px);
}
```

> 该方案实现的核心是 `calc()` 函数，关于 `calc()` 函数，可以通过我另一篇文章 [【不一样的CSS】一文让你了解CSS中的各种单位](https://juejin.cn/post/6964025107685572644) 鼠标滚动的最后进行了解。

### 定位+定宽+定高实现水平垂直居中方案(二)

步骤如下：

1. 使子元素相对于容器元素定位
2. 子元素开启绝对定位
3. 设置该元素的偏移量，值为 50%
4. 通过外边距 -值 的方式将元素移动回去

完成CSS代码如下：

```css
.parent {
  /* 1. 使子元素相对于本元素定位 */
  position: relative;
}
.child {
  /* 2. 开启绝对定位 */
  position: absolute;
  /* 3. 设置该元素的偏移量，值为 50% */
  left: 50%;
  top: 50%;
  margin-left: -150px;
  margin-top: -150px;
}
```

### 定位+定宽+定高实现水平垂直居中方案(三)

步骤如下：

1. 使子元素相对于容器元素定位
2. 子元素开启绝对定位
3. 将子元素拉满整个容器
4. 通过 `margin:auto` 实现水平垂直居中

完成CSS代码如下：

```css
.parent {
  /* 1. 使子元素相对于本元素定位 */
  position: relative;
}
.child {
  /* 2. 开启绝对定位 */
  position: absolute;
  /* 3. 将子元素拉满整个容器 */
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  /* 4. 通过 margin:auto 实现水平垂直居中 */
  margin: auto;
}
```

### 定位+transform实现水平垂直居中

步骤如下：

1. 使子元素相对于容器元素定位
2. 子元素开启绝对定位
3. 设置该元素的偏移量，值为 50%
4. 通过 `translate` 反向偏移的方式，实现居中

完成CSS代码如下：

```css
.parent {
  /* 1. 使子元素相对于本元素定位 */
  position: relative;
}
.child {
  /* 2. 开启绝对定位 */
  position: absolute;
  /* 3. 设置该元素的偏移量，值为 50%*/
  left: 50%;
  top: 50%;
  /* 通过translate反向偏移的方式，实现居中 */
  transform: translate(-50%, -50%);
}
```

> 实现原理：`left` 相对于父级进行左偏偏移，`50%` 也就是父级宽度 500px 的一半，也就是 `250px`; `translate` 同样是进行偏移，与之不同的是，该方式参照的是当前元素，X轴偏移 `-50%` 的话，就是 `300px` 的 `50%`，也就是 `150px`，最终 子元素距离父元素边距为 `100px`。即 `100px` `300px`  `100px`  最终实现了居中。

### Flex 方案

步骤如下：

1. 将元素设置为 Flex 布局
2. 通过 `justify-content: center` 以及 `align-items: center` 实现或者 `margin: auto;` 实现。

完成CSS代码如下：

```css
.parent {
  /* 1. 将元素设置为 Flex 布局 */
  display: flex;
  /* 2. 通过 justify-content 以及 align-items: center 实现 */
  /* justify-content: center;
  align-items: center; */
}
.child {
  /* 或者通过 margin auto 实现 */
  margin: auto;
}
```

> 关于 Flex 布局的详细用法请参考 [点我进入](https://juejin.cn/post/6963250638214365220)

### Grid 方案

Grid 的实现方案比较多也比较简单，具体代码如下：

```css
.parent {
  /* 1. 元素设置为Grid 元素 */
  display: grid;
  /* 通过 items 属性实现*/
  /* align-items: center; */
  /* justify-items: center; */
  /* items 的缩写 */
  /* place-items: center; */

  /* 或者通过 content 属性 */
  /* align-content: center; */
  /* justify-content: center; */
  /* content 的缩写 */
  /* place-content: center; */
}
.child {
  /* 或者通过 margin auto 实现 */
  /* margin: auto; */
  /* 或者通过 self 属性 */
  /* align-self: center;
  justify-self: center; */
  /* self 的缩写 */
  place-self: center;
}
```

> 关于 Grid 布局的详细用法请参考 [点我进入](https://juejin.cn/post/6963252773202690055)

## 总结

![](http://img.seecode.cc//picgo/%E5%AE%9E%E7%8E%B0%E6%B0%B4%E5%B9%B3%E5%9E%82%E7%9B%B4%E5%B8%83%E5%B1%80%E7%9A%84%207%20%E7%A7%8D%E6%96%B9%E5%BC%8F.png)