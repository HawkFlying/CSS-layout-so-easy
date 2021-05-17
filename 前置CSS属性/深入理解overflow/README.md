# 【不一样的CSS】深入理解 overflow (溢出要学会处理)


## 写在前面

对 CSS 布局掌握程度决定你在 Web 开发中的开发页面速度。随着 Web 技术的不断革新，实现各种布局的方式已经多得数不胜数了。

最近利用碎片时间，大概用了半个月的时间整理了一个系列，本系列文章总结了 CSS 中的各种布局，以及实现方式及其常用技巧。让你通过该系列文章对 CSS 布局有一个新的认识。

该系列的导航帖[点我进入](https://juejin.cn/post/6963251091035291656/)，里面可以快速跳转到你想了解的文章(建议收藏)

## overflow 的基本属性

### 概述

`overflow` 属性用于当一个元素太大而无法适应父级容器的大小时需要做什么。该属性有四个常用的值：

- `visible`: 默认值。内容不会回修剪，可以呈现在元素框之外。
- `hidden`: 如果内容超出父级容器，超出部分将会被隐藏
- `scroll`: 无论是否超出容器，都会出现一个滚动条。
- `auto`: 如果没有超出容器的显示，将会正常显示，如果超出，将会出现一个滚动条。

示例代码如下：

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta http-equiv="X-UA-Compatible" content="IE=edge" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>overflow基本属性</title>
        <style>
            .main {
                width: 1200px;
                display: flex;
                justify-content: space-between;
                margin: 0 auto;
            }
            .container {
                height: 210px;
                width: 260px;
                background-color: #70a1ff;
            }
            img {
                width: 300px;
                opacity: 0.7;
            }
            .visible {
                /* 超出部分溢出显示 */
                overflow: visible;
            }
            .hidden {
                /* 超出部分被隐藏 */
                overflow: hidden;
            }
            .scroll {
                /* 水平垂直都出现滚动条，可以滚动显示*/
                overflow: scroll;
            }
            .auto {
                /* 超出部门出现滚动条，未超出部门可以正常显示 */
                overflow: auto;
            }
        </style>
    </head>
    <body>
        <div class="main">
            <div class="container visible"><img src="../image/img.jpg" /></div>
            <div class="container hidden"><img src="../image/img.jpg" /></div>
            <div class="container scroll"><img src="../image/img.jpg" /></div>
            <div class="container auto"><img src="../image/img.jpg" /></div>
        </div>
    </body>
</html>
```

执行结果如下所示：

![image-20210516141540978](http://img.seecode.cc//picgo/image-20210516141540978.png)

### overflow-x 与 overflow-y

`overflow-x` 与 `overflow-y` 可以分别设置水平和垂直时溢出的部分该怎么怎么处理。

**值得注意的是**，如果 `overflow-x` 与 `overflow-y` 的值相同，结果等同于 `overflow`；如果  `overflow-x` 与 `overflow-y` 的值不相同，且其中一个属性的值被赋予 `visible`，另外一个被赋予一个 非 `visible` 的值，第一个被赋予 `visible` 的值会自动变为 `auto`。

### 使用 overflow 的前提

为使 `overflow `有效果，容器必须满足以下条件：

1. `dispaly` 的值非 `inline` 。
2. 具有尺寸限制。(`width` / `height` / `max-width` / `max-height` / `absolute`拉伸 )
3. 对于单元格 `td` 等,还需要 `table` 为 `table-layout: fixed ` 才可以。

## overflow 与滚动条

### 滚动条出现的条件

滚动条出现条件主要分为以下两种：

1. 使用 `overflow` 属性出现的滚动条
2. HTML 元素自带的，例如 `<html>` 和 `<textarea>` 属性。

> **值得注意的是**，默认滚动条是来自于 `<html>` 元素而不是 `<body>` 元素。滚动条也会占用用容器的可用宽度或者高度

### 获取滚动条高度

获取滚动条高度的 JavaScript 代码如下：

```js
let st = document.documentElement.scrollTop || document.body.scrollTop;
```

### 自定义滚动条

`webkit` 内核提供了以下适用于 `webkit` 内核的浏览器自定义滚动条的样式的属性，具体如下：

- `::-webkit-scrollbar` — 整个滚动条.
- `::-webkit-scrollbar-button` — 滚动条上的按钮 (上下箭头).
- `::-webkit-scrollbar-thumb` — 滚动条上的滚动滑块.
- `::-webkit-scrollbar-track` — 滚动条轨道.
- `::-webkit-scrollbar-track-piece` — 滚动条没有滑块的轨道部分.

示例代码如下所示：

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta http-equiv="X-UA-Compatible" content="IE=edge" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>自定义滚动条</title>
        <style>
            body {
                height: 1000px;
            }
            /* 整个滚动条. */
            ::-webkit-scrollbar {
                width: 12px;
            }
            /*  滚动条轨道 */
            ::-webkit-scrollbar-track {
                background-color: #ff7875;
            }
            /*  滚动条上的滚动滑块 */
            ::-webkit-scrollbar-thumb {
                background-color: #ffc069;
                border-radius: 6px;
            }
        </style>
    </head>
    <body></body>
</html>
```

效果如下所示：

![image-20210516150025496](http://img.seecode.cc//picgo/image-20210516150025496.png)

## 块格式化上下文

块格式化上下文英文 BFC（Block Formatting Contexts ）。。是Web页面中盒模型布局的CSS渲染模式，指一个独立的渲染区域或者说是一个独立的独立容器。

### BFC 布局规则

1. 内部的元素会在垂直方向，从顶部开始一个接一个地放置。 
2. 元素垂直方向的距离由 `margin` 决定。属于同一个 BFC 的两个相邻 元素的 `margin` 会发生叠加
3. 都是从最左边开始的。每个元素的 `margin box` 的左边，与包含块 `border box` 的左边(对于从左往右的格式化，否则
   相反)。即使存在浮动也是如此
4. BFC 的区域不会与 `float box` 叠加。 
5. BFC 就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素，反之亦然。 
6. 计算 BFC 的高度时，浮动元素也参与计算（当 BFC 内部有浮动时，为了不影响外部元素的布局，BFC 计算高度时会包
   括浮动元素的高度）

### 创建 BFC

下列方式会创建块格式化上下文：

- 根元素（`<html>`）
- 浮动元素（元素的 `float` 不是 `none`）
- 绝对定位元素（元素的 `position` 为 `absolute` 或 `fixed`）
- 行内块元素（元素的 `display` 为 `inline-block`）
- 表格单元格（元素的 `display` 为 `table-cell`，HTML 表格单元格默认为该值）
- 表格标题（元素的 `display` 为 `table-caption`，`HTML`表格标题默认为该值）
- `overflow` 计算值不为 `visible` 的块元素
- `display` 值为 `flow-root` 的元素
- 弹性元素（`display` 为 `flex` 或 `inline-flex` 元素的直接子元素）
- 网格元素（`display` 为 `grid` 或 inline-grid 元素的直接子元素）

## 依赖于 overflow 的 CSS 属性

所谓依赖于 `overflow` 的 CSS 属性，就是不设置为 `overflow ` 属性的值为 `visible` 时，该属性是失效的，依赖于 overflow 的 CSS 属性非主要有两个:

### 1. resize 属性

该属性用于设定一个元素的是否可调整大小。

该属性具有如下几个值：

- `none`: 默认值，元素不能被用户缩放。
- `both`: 允许用户在水平和垂直方向上调整元素的大小。
- `horizontal`: 允许用户在水平方向上调整元素的大小。
- `vertical`: 允许用户在垂直方向上调整元素的大小。

### 2. text-overflow 属性

该属性用于指定当文本溢出时的操作。

该属性具有如下几个值：

- `clip`: 默认值"在内容区域的极限处截断文本
- `ellipsis`: 用 `...` 来表示被截断的文本
- `<string>`: 该字符串内容将会被添加在内容区域中，如果空间太小，该字符串也会被截断。

