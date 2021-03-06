# Grid 布局


> **若彼岸繁华落尽，谁陪我看落日流年**

## 写在前面

对 CSS 布局掌握程度决定你在 Web 开发中的开发页面速度。随着 Web 技术的不断革新，实现各种布局的方式已经多得数不胜数了。

最近利用碎片时间，大概用了半个月的时间整理了一个系列，本系列文章总结了 CSS 中的各种布局，以及实现方式及其常用技巧。让你通过该系列文章对 CSS 布局有一个新的认识。

该系列的导航帖[点我进入](https://juejin.cn/post/6963251091035291656/)，里面可以快速跳转到你想了解的文章(建议收藏)

Grid 布局(*网格布局*)是 CSS 最新的也是最强大的一种布局方案。

### 文章概述

由于篇幅较长(请谨慎阅读)，下图涵盖了本篇文章的主要知识点

![](http://img.seecode.cc//picgo/Grid%20%E5%B8%83%E5%B1%80%E6%80%BB%E7%BB%93.png)

正文开始

### 基本概念

**网格**：网格是一组相交的水平线和垂直线，它定义了网格的列和行，我们可以将网格元素放置在与这些行和列相关的位置上。

**网格容器**: 所谓的网格容器就是所有网格项的父元素，其 `display` 的值 为 `grid`。

**网格项**: 即网格容器的子元素(不包含子元素的子元素)。

**网格线**: 即组成网格项的分界线，Grid 会为我们创建编号的网格线来让我们来定位每一个网格元素。 例如下面这个三列两行的网格中，就拥有四条纵向的网格线。

![](http://img.seecode.cc//picgo/1_diagram_numbered_grid_lines.png)

**网格轨道**: 一个网格轨道就是网格中任意两条线之间的空间。在下图中你可以看到一个高亮的轨道——网格的第一个行轨道。

![1_Grid_Track](http://img.seecode.cc//picgo/1_Grid_Track.png)

**网格单元**: 两个相邻的列网格线和两个相邻的行网格线组成的是网格单元。在下面的图中，我会将第一个网格单元作高亮处理。

![1_Grid_Cell](http://img.seecode.cc//picgo/1_Grid_Cell.png)

**网格区域**: 四个网格线包围的总空间。下图高亮的网格区域扩展了2列以及2行。

![1_Grid_Area](http://img.seecode.cc//picgo/1_Grid_Area.png)

**fr (单位)**: 剩余空间分配数。用于在一系列长度值中分配剩余空间，如果多个已指定了多个部分，则剩下的空间根据各自的数字按比例分配。

## 容器属性

### display 属性

`display` 属性用于指定一个容器采用网格容器。语法如下：

```css
.container {
  display: grid | inline-grid;
  /*
  	* grid: 生成块级网格
  	* inline-grid: 生成行内网格
  */
}
```



> **值得注意的是**设为网格容器时，网格项的 `float`、`display: inline-block`、`display: table-cell`、`vertical-align` 和 `column-*` 等设置都将失效。

示例代码如下所示：

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta http-equiv="X-UA-Compatible" content="IE=edge" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>display 属性</title>
        <link rel="stylesheet" href="./init.css" />
        <style>
            .container {
                display: grid;
                grid-template-columns: 1fr 1fr 1fr 1fr;
            }
        </style>
    </head>
    <body>
        <div class="container">
            <div class="item1 item">1</div>
            <div class="item2 item">2</div>
            <div class="item3 item">3</div>
            <div class="item4 item">4</div>
            <div class="item5 item">5</div>
            <div class="item6 item">6</div>
            <div class="item7 item">7</div>
            <div class="item8 item">8</div>
        </div>
    </body>
</html>

```

> `init.css`
>
> ```css
> body { margin: 0; padding: 20px; }
> .item { height: 300px; width: 300px; line-height: 300px; text-align: center; font-size: 140px; }
> .item1 { background-color: #e6005c; }
> .item2 { background-color: #777bce; }
> .item3 { background-color: #c9780c; }
> .item4 { background-color: #fffae8; }
> .item5 { background-color: #ce3b3b; }
> .item6 { background-color: #e666ff; }
> .item7 { background-color: #f4ea20; }
> .item8 { background-color: #b4a4ca; }
> ```

执行结果如下：

![image-20210508214141493](http://img.seecode.cc//picgo/image-20210508214141493.png)

### grid-template

#### 1. `grid-template-columns` 属性 和 `grid-template-rows` 属性

`grid-template-columns` 属性 和 `grid-template-rows` 属性,用来指定网格的列宽以及行高，以空格分隔，语法结构如下：

```css
.container {
	grid-template-columns: <track-size> ... | <line-name> <track-size> ...;
  grid-template-rows: <track-size> ... | <line-name> <track-size> ...;
}
```

**属性值**

- 轨道大小(*track-size*): 可以使用任意 css 长度、百分比、或者分数、或者 `fr` 单位。
- 网格线名字(*line-name*): 可以选择任意名字

示例代码如下：

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta http-equiv="X-UA-Compatible" content="IE=edge" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>grid-template 属性</title>
        <link rel="stylesheet" href="./init.css" />
        <style>
            .container {
                background-color: #fef3c9;
                display: grid;
                width: 1600px;
                height: 800px;
                margin: 0 auto;
                grid-template-columns: 300px auto 300px 20%;
                grid-template-rows: 1fr 1fr;
            }
            .item2,
            .item3,
            .item6,
            .item7 {
                width: auto;
            }
        </style>
    </head>
    <body>
        <div class="container">
            <div class="item1 item">1</div>
            <div class="item2 item">2</div>
            <div class="item3 item">3</div>
            <div class="item4 item">4</div>
            <div class="item5 item">5</div>
            <div class="item6 item">6</div>
            <div class="item7 item">7</div>
            <div class="item8 item">8</div>
        </div>
    </body>
</html>
```

执行结果如下所示：

![image-20210508222658446](http://img.seecode.cc//picgo/image-20210508222658446.png)

#### 2. 网格线的名称

`grid-template-columns` 属性 和 `grid-template-rows` 属性 里面，还可以使用方括号 `[]` ，指定每一根网格线的名字，方便以后的引用。

示例代码如下：

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta http-equiv="X-UA-Compatible" content="IE=edge" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>网格线的名称</title>
        <link rel="stylesheet" href="./init.css" />
        <style>
            .container {
                background-color: #fef3c9;
                display: grid;
                width: 1600px;
                height: 700px;
                margin: 0 auto;
                grid-template-columns: [c1] 320px [c2] 3fr [c3] 2fr [c4] 20% [c5];
                grid-template-rows: [r1] 1fr [r2] 1fr [r3];
            }
        </style>
    </head>
    <body>
        <div class="container">
            <div class="item1 item">1</div>
            <div class="item2 item">2</div>
            <div class="item3 item">3</div>
            <div class="item4 item">4</div>
            <div class="item5 item">5</div>
            <div class="item6 item">6</div>
            <div class="item7 item">7</div>
            <div class="item8 item">8</div>
        </div>
    </body>
</html>

```

效果图如下：

![image-20210508224148105](http://img.seecode.cc//picgo/image-20210508224148105.png)

#### 3. `repeat()` 函数

有的时候，我们需要编写同样的值，尤其是在网格多的时候，就显得尤为的麻烦，这个时候 `repeat()` 函数就帮助我们解决了这个问题。

该函数适用于 CSS Grid 属性中 `grid-template-columns` 和 `grid-template-rows`.语法规则如下：

```css
.container {
  grid-template-columns: repeat(repeat, value);
}
```

**属性值**：

- `repeat`: 表示重复的次数

  **可选值**：

  - `number`: 整数，确定确切的重复次数
  - `auto-fill`: 以网格项为准自动填充
  - `auto-fit`: 以网格容器为准自动填充

- `value`: 表示重复的值

  **可选值**：

  - `length`: 非负长度
  - `percentage`: 相对于列轨道中网格容器的内联大小的非负百分比，以及行轨道中网格容器的块长宽。
  - `flex`: 单位为 fr 的非负维度，指定轨道弹性布局的系数值。

示例代码如下所示：

```css
.container {
  grid-template-columns: 1fr 1fr 1fr 1fr 1fr 1fr;
  /* 等于 */
  grid-template-columns: repeat(6, 1fr);
}
```

#### `4. minmax()` 函数

`minmax()` 函数定义了一个长宽范围的闭区间，该函数适用于 CSS Grid 属性中 `grid-template-columns` 和 `grid-template-rows` 语法规则如下：

```css
.container {
  grid-template-columns: minmax(minValue, maxValue);
}
```

**可选值**：

- `length`: 非负长度
- `percentage`: 相对于列轨道中网格容器的内联大小的非负百分比，以及行轨道中网格容器的块长宽。
- `flex`: 单位为 fr 的非负维度，指定轨道弹性布局的系数值。

示例代码如下所示：

```css
.container {
  grid-template-columns: minmax(100px, 400px);
}
```

> 最小`100px`最大`400px`

#### `5. grid-template-areas` 属性

通过引用 `grid-area` 属性指定的网格区域的名称来定义网格模板。语法结构如下：

```css
.contaienr {
  grid-template-areas: none |
    'grid-area-name | . grid-area-name | . grid-area-name | . ...'
    'grid-area-name | . grid-area-name | . grid-area-name | . ...'
}
```

**属性值**

- `grid-area-name`: 使用 `grid-area` 属性设置的网格区域名称
- `.` : 点号表示一个空网格单元
- `none`: 没有定义网格区域。

示例代码如下

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta http-equiv="X-UA-Compatible" content="IE=edge" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>grid-template-areas 属性</title>
        <link rel="stylesheet" href="./init.css" />
        <style>
            .container {
                background-color: #fef3c9;
                /* 1. 设置容器元素为 网格容器  */
                display: grid;
                width: 1600px;
                height: 800px;
                margin: 0 auto;
                /* 划分区域，当前区域为 三行 两列 */
                grid-template-areas:
                    'header header'
                    'nav main'
                    'footer footer';
                /* 3. 设置各区域的宽度和高度 */
                grid-template-columns: 300px 1fr;
                grid-template-rows: 200px auto 200px;
            }
            .item {
                width: auto;
                height: 200px;
                line-height: 200px;
            }
            .header {
                /* 4. 指定当前元素所在的区域位置，从 grid-template-areas 选取值 */
                grid-area: header;
            }
            .nav {
                /* 4. 指定当前元素所在的区域位置，从 grid-template-areas 选取值 */
                grid-area: nav;
            }
            .main {
                /* 4. 指定当前元素所在的区域位置，从 grid-template-areas 选取值 */
                grid-area: main;
            }
            .footer {
                /* 4. 指定当前元素所在的区域位置，从 grid-template-areas 选取值 */
                grid-area: footer;
            }
            .nav,
            .main {
                height: auto;
                line-height: 400px;
            }
        </style>
    </head>
    <body>
        <div class="container">
            <div class="item1 item header">header</div>
            <div class="item2 item nav">nav</div>
            <div class="item3 item main">main</div>
            <div class="item4 item footer">footer</div>
        </div>
    </body>
</html>
```

执行结果如下：

![image-20210509134334506](http://img.seecode.cc//picgo/image-20210509134334506.png)

> **值得注意的是**区域的命名会影响到网格线。每个区域的起始网格线，会自动命名为 `区域名-start` ，终止网格线自动命名为 `区域名-end` 。
>
> 比如，区域名为 `header` ，则起始位置的水平网格线和垂直网格线叫做 `header-start` ，终止位置的水平网格线和垂直网格线叫做 `header-end` 。

#### grid-template 属性

该属性是 `grid-template-rows` 、 `grid-template-columns` 和 `grid-template-areas` 属性的简写。语法结构如下所示：

```css
.container {
  grid-template: none | [ <'grid-template-rows'> / <'grid-template-columns'> ] | [ <line-names>? <string> <track-size>? <line-names>? ]+ [ / <explicit-track-list> ]? ;
}
```

**属性值**

- `none`: 将三个属性都设置为初始值，也就是一行一列一区域(了解)。
- `grid-template-rows / grid-template-columns`: 把  `grid-template-rows` 和 `grid-template-columns` 设置为指定值，于此同时，设置 `grid-template-areas` 为 `none`
- `[ <line-names>? <string> <track-size>? <line-names>? ]+ [ / <explicit-track-list> ]?`: 设 `grid-template-areas` 为列得 `<string>` 、 `grid-template-columns` 为 `<explicit-track-list>` （默认为`none`）、 `grid-template-rows` 为 `<track-size>`（默认为`auto`）并拼接尺寸前后所定义之行。

示例代码如下所示：

```css
grid-template-areas:
            'header header'
            'nav main'
            'footer footer';
grid-template-columns: 300px 1fr;
grid-template-rows: 200px auto 200px;

/* 简写如下 */

grid-template:
              [row1-start] 'header header' 200px [row1-end]
              [row2-start] 'nav main' auto [row2-end]
              [row3-start] 'footer footer' 200px [row3-end]
              / 300px 1fr;
```

> 上面两个代码最终的执行效果是相同的。

不过按照代码的可读性来看，**并不推荐使用简写方式**

### gap 属性

`gap` 属性是 `row-gap` 和 `column-gap` 属性的简写。

该属性用于指定网格线的大小，可以想象为设置列 / 行之间的间距的宽度。语法结构如下：

```css
.contianer {
  column-gap: <line-size>;
  row-gap: <line-size>;
  /* 如果省略第二个值，浏览器认为第二个值等于第一个值。 */
  gap: <line-size> <line-size>;
}
```

**属性值**

- `<line-size>`: 长度值，例如 `20px`。

示例代码如下(基于上面那一段代码来写)：

```css
.container {
  /* 写法 1 */
  column-gap: 10px;
  row-gap: 10px;
  /* 写法 2 */
  gap: 10px 10px;
  /* 写法 3 */
  gap: 10px;
}
```

执行结果如下：

![image-20210509150302498](http://img.seecode.cc//picgo/image-20210509150302498.png)

> 值得注意的是 `gap` 、`column-gap` 和 `row-gap`  属性在之前是有 `grid-` 的前缀的，即 `grid-gap` 、`grid-column-gap` 和 `grid-row-gap`。在使用的过程中，为了保证有效，我们可以这两个属性一起写。

### items 属性

#### 1. `align-items` 属性

沿着**列轴**对齐网格内的内容。语法结构如下：

```css
.container {
  align-items: start | end | center | stretch;
}
```

**属性值**：

- `start`: 内容与网格区域的顶端对齐
- `end`: 内容与网格区域的底部对齐
- `center`: 内容位于网格区域的垂直中心位置
- `stretch`: 内容高度占据整个网格区域空间(默认值)

#### 2. `justify-items` 属性

沿着**行轴**对齐网格内的内容。语法结构如下：

```css
.container {
  justify-items: start | end | center | stretch;
}
```

**属性值**：

- `start`: 内容与网格区域的左端对齐
- `end`: 内容与网格区域的右部对齐
- `center`: 内容位于网格区域的水平中心位置
- `stretch`: 内容宽度占据整个网格区域空间(默认值)

#### 3. `place-items` 属性

`place-items` 是一个简写属性，使用此属性可以同时设置**列轴**对齐和**行轴**对齐，语法结构如下所示：

```css
.container {
  place-items: align-items justify-items;
}
```

如果只写一个值，第二个值默认与第一个值相同。

示例代码如下所示：

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta http-equiv="X-UA-Compatible" content="IE=edge" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>grid-template 属性</title>
        <link rel="stylesheet" href="./init.css" />
        <style>
            .container {
                background-color: #fef3c9;
                display: grid;
                width: 1600px;
                height: 800px;
                margin: 0 auto;
                grid-template-columns: 1fr 1fr 1fr 1fr;
                grid-template-rows: 1fr 1fr;
                /* align-items 属性，控制网格项垂直对齐方式 */
                align-items: end;
                /* justify-items 属性，控制网格项水平对齐方式 */
                justify-items: center;
                /* 
                    place-items 属性，是 align-items 属性 和 justify-items 的简写形式 
                    如果只写一个值，第二个值默认与第一个值相同
                */
                place-items: end center;
            }
            .item {
                height: 200px;
                width: 200px;
                line-height: 200px;
            }
        </style>
    </head>
    <body>
        <div class="container">
            <div class="item1 item">1</div>
            <div class="item2 item">2</div>
            <div class="item3 item">3</div>
            <div class="item4 item">4</div>
            <div class="item5 item">5</div>
            <div class="item6 item">6</div>
            <div class="item7 item">7</div>
            <div class="item8 item">8</div>
        </div>
    </body>
</html>

```

执行结果如下所示：

![image-20210509165817094](http://img.seecode.cc//picgo/image-20210509165817094.png)

### content 属性

#### 1. `align-content ` 属性

设置网格容器内的网格沿着**列轴**对齐网格的对齐方式。语法结构如下：

```css
.container {
  align-content : start | end | center | stretch | space-around | space-between | space-evenly;
}
```

**属性值**：

- `start`: 网格与网格容器的顶部对齐
- `end`: 网格与网格容器的底部对齐
- `center`: 网格与网格容器的垂直中间对齐
- `stretch`: 调整网格项的大小，让高度填充整个网格容器
- `space-around`:在网格项之间设置均等高度的空白间隙,其外边缘间隙大小为中间空白间隙宽度的一伴
- `space-between`:在网格项之间设置均等高度空白间隙，其外边缘无间隙
- `space-evenly`:在每个网格项之间设置均等高度的空白间隙,包括外边缘

#### 2. `justify-content ` 属性

设置网格容器内的网格沿着**行轴**对齐网格的对齐方式。语法结构如下：

```css
.container {
  align-content : start | end | center | stretch | space-around | space-between | space-evenly;
}
```

**属性值**：

- `start`: 网格与网格容器的左部对齐
- `end`: 网格与网格容器的右部对齐
- `center`: 网格与网格容器的水平中间对齐
- `stretch`: 调整网格项的大小，让高度填充整个网格容器
- `space-around`:在网格项之间设置均等宽度的空白间隙,其外边缘间隙大小为中间空白间隙宽度的一伴
- `space-between`:在网格项之间设置均等宽度空白间隙，其外边缘无间隙
- `space-evenly`:在每个网格项之间设置均等宽度的空白间隙,包括外边缘

#### 3. `place-content` 属性

`place-content` 属性是 `align-content` 和 `justify-content` 的简写属性，，语法结构如下所示：

```css
.container {
  place-content: align-content justify-content;
}
```

如果只写一个值，第二个值默认与第一个值相同。

示例代码如下所示：

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta http-equiv="X-UA-Compatible" content="IE=edge" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>content 属性</title>
        <link rel="stylesheet" href="./init.css" />
        <style>
            .container {
                background-color: #fffae8;
                display: grid;
                width: 1600px;
                height: 800px;
                margin: 0 auto;
                grid-template-columns: 300px 300px 300px 300px;
                grid-template-rows: 300px 300px;
                place-items: center;
                /* align-content 属性，控制网格容器垂直对齐方式 */
                align-content: space-around;
                /* justify-content 属性，控制网格容器水平对齐方式 */
                justify-content: space-around;
                /* 
                    place-content 属性，是 align-content 属性 和 justify-content 的简写形式 
                    如果只写一个值，第二个值默认与第一个值相同
                */
                place-content: space-around;
            }
            .item {
                height: 200px;
                width: 200px;
                line-height: 200px;
            }
        </style>
    </head>
    <body>
        <div class="container">
            <div class="item1 item">1</div>
            <div class="item2 item">2</div>
            <div class="item3 item">3</div>
            <div class="item4 item">4</div>
            <div class="item5 item">5</div>
            <div class="item6 item">6</div>
            <div class="item7 item">7</div>
            <div class="item8 item">8</div>
        </div>
    </body>
</html>

```

执行结果如下图所示：

![image-20210509174704749](http://img.seecode.cc//picgo/image-20210509174704749.png)

### grid-auto 属性

#### 1. `grid-auto-columns` 属性 和 `grid-auto-rows` 属性

该组属性指定自动生成的网络轨道(隐式网络轨道)的大小

> 隐式网络轨道在显示的定位超出指定网格范围的行或列时被创建。

它们的写法与`grid-template-columns`和`grid-template-rows`相同。如果不指定此属性，浏览器完全根据单元格内容的大小，决定新增网格的列宽和行高。

#### 2. `grid-auto-flow` 属性

该属性控制自动布局算法的工作方式。语法结构如下

```css
.container {
  grid-auto-flow: [ row | column ] || dense
}
```

**属性值**：

- `row`: 告诉自动布局算法依次填充每行，根据需要添加新行
- `column`: 告诉自动布局算法依次填充每列，根据需要添加新列
- `dense`: 告诉自动布局算法，如果后面出现较小的网格项,则尝试在网格中填充空洞

### grid 属性

grid 是一个CSS简写属性，可以用来设置以下属性：

显式网格属性 `grid-template-rows`、`grid-template-columns` 和 `grid-template-areas`，
隐式网格属性 `grid-auto-rows`、`grid-auto-columns` 和  `grid-auto-flow`，
间距属性 `grid-column-gap` 和 `grid-row-gap`。

语法结构如下：

```css
.contaienr {
  grid: <'grid-template'> | <'grid-template-rows'> / [ auto-flow && dense? ] <'grid-auto-columns'>? | [ auto-flow && dense? ] <'grid-auto-rows'>? / <'grid-template-columns'>;
}
```

> 该语法不易读，在实际开发中并推荐使用该语法。

## 网格项上的属性

### start / end

#### 1. `grid-column-start` 属性、`grid-column-end` 属性、`grid-row-start` 属性、`grid-row-end` 属性

网格项的位置可以通过 `grid-column-start` 、`grid-column-end` 、`grid-row-start` 、`grid-row-end` 这四个属性进行确定。

语法结构如下：

```css
.item {
  /* 左边框所在的垂直网格线 */
  grid-column-start: <number> | <name> | span <number> | span <name> | auto;
  /* 右边框所在的垂直网格线 */
  grid-column-end: <number> | <name> | span <number> | span <name> | auto;
  /* 上边框所在的垂直网格线 */
  grid-row-start: <number> | <name> | span <number> | span <name> | auto;
  /* 下边框所在的垂直网格线 */
  grid-row-end: <number> | <name> | span <number> | span <name> | auto;
}
```

**属性值**

- `<number>`: 用数字来指代相应编号的网格线。
- `<name>`:使用网格线名称来指代相应命名的网格线。
- `span <number>`: 网格项将跨越指定数量的网格轨道
- `span <name>`: 网格项将跨越一些轨道，直到碰到指定命名的网格线
- `auto`: 自动布局，或者自动跨越，或者跨越一个默认的轨道

示例代码如下所示：

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta http-equiv="X-UA-Compatible" content="IE=edge" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>start / end</title>
        <link rel="stylesheet" href="./init.css" />
        <style>
            .container {
                background-color: #fffae8;
                display: grid;
                width: 920px;
                height: 820px;
                margin: 0 auto;
                grid-template-columns: [c1] 300px [c2] 300px [c3] 300px [c4];
                grid-template-rows: [r1] 200px [r2] 200px [r3] 200px [r4];
                gap: 10px;
            }
            .item {
                line-height: 200px;
                height: 200px;
            }
            .item1 {
                /* 1. 纯数字写法 */
                grid-column-start: 1;
                grid-column-end: 3;
                grid-row-start: 1;
                grid-row-end: 3;

                /* 2. 纯名字写法 */
                grid-column-start: c1;
                grid-column-end: c3;
                grid-row-start: r1;
                grid-row-end: r3;

                /* 3. 数字 + span 数字 */
                grid-column-start: 1;
                grid-column-end: span 2;
                grid-row-start: 1;
                grid-row-end: span 2;

                /* 4. 数字 + span 名字*/
                grid-column-start: 1;
                grid-column-end: span c3;
                grid-row-start: 1;
                grid-row-end: span r3;
                width: auto;
                height: auto;
            }
        </style>
    </head>
    <body>
        <div class="container">
            <div class="item1 item">1</div>
            <div class="item2 item">2</div>
            <div class="item3 item">3</div>
            <div class="item4 item">4</div>
            <div class="item5 item">5</div>
            <div class="item6 item">6</div>
            <div class="item7 item">7</div>
            <div class="item8 item">8</div>
        </div>
    </body>
</html>

```

执行结果如下所示：

![image-20210509194719268](http://img.seecode.cc//picgo/image-20210509194719268.png)

> **值得注意的是**如果没有声明 `end` 则默认将跨域一个轨道
>
> 示例代码如下：
>
> ```html
> <!DOCTYPE html>
> <html lang="en">
>     <head>
>         <meta charset="UTF-8" />
>         <meta http-equiv="X-UA-Compatible" content="IE=edge" />
>         <meta name="viewport" content="width=device-width, initial-scale=1.0" />
>         <title>不声明 end</title>
>         <link rel="stylesheet" href="./init.css" />
>         <style>
>             .container {
>                 background-color: #fffae8;
>                 display: grid;
>                 width: 920px;
>                 height: 620px;
>                 margin: 0 auto;
>                 grid-template-columns: [c1] 300px [c2] 300px [c3] 300px [c4];
>                 grid-template-rows: [r1] 200px [r2] 200px [r3] 200px [r4];
>                 gap: 10px;
>             }
>             .item { line-height: 200px; height: 200px; }
>             .item1 {
>                 grid-column-start: 2;
>                 width: auto;
>                 height: auto;
>             }
>         </style>
>     </head>
>     <body>
>         <div class="container">
>             <div class="item1 item">1</div>
>             <div class="item2 item">2</div>
>             <div class="item3 item">3</div>
>             <div class="item4 item">4</div>
>             <div class="item5 item">5</div>
>             <div class="item6 item">6</div>
>             <div class="item7 item">7</div>
>             <div class="item8 item">8</div>
>         </div>
>     </body>
> </html>
> 
> ```
>
> 效果图如下：
>
> ![image-20210509194917673](http://img.seecode.cc//picgo/image-20210509194917673.png)
>
> **网格项如果相互重叠，可以使用 z-index 来控制它们的堆叠顺序**
>
> 示例代码如下：
>
> ```html
> <!DOCTYPE html>
> <html lang="en">
>     <head>
>         <meta charset="UTF-8" />
>         <meta http-equiv="X-UA-Compatible" content="IE=edge" />
>         <meta name="viewport" content="width=device-width, initial-scale=1.0" />
>         <title>堆叠</title>
>         <link rel="stylesheet" href="./init.css" />
>         <style>
>             .container {
>                 background-color: #fffae8;
>                 display: grid;
>                 width: 920px;
>                 height: 620px;
>                 margin: 0 auto;
>                 grid-template-columns: [c1] 300px [c2] 300px [c3] 300px [c4];
>                 grid-template-rows: [r1] 200px [r2] 200px [r3] 200px [r4];
>                 gap: 10px;
>             }
>             .item { line-height: 200px; height: 200px; }
>             .item1, .item2 { width: auto; height: auto; }
>             .item1 {
>                 grid-column-start: 1;
>                 grid-column-end: 3;
>                 grid-row-start: 1;
>                 grid-row-end: 3;
>                 /* 通过 z-index 控制堆叠顺序 */
>                 z-index: 2;
>             }
>             .item2 {
>                 grid-column-start: 2;
>                 grid-column-end: 4;
>                 grid-row-start: 1;
>                 grid-row-end: 3;
>             }
>         </style>
>     </head>
>     <body>
>         <div class="container">
>             <div class="item1 item">1</div>
>             <div class="item2 item">2</div>
>             <div class="item3 item">3</div>
>             <div class="item4 item">4</div>
>             <div class="item5 item">5</div>
>             <div class="item6 item">6</div>
>             <div class="item7 item">7</div>
>             <div class="item8 item">8</div>
>         </div>
>     </body>
> </html>
> 
> ```
>
> 效果图如下：
>
> ![image-20210509195250297](http://img.seecode.cc//picgo/image-20210509195250297.png)

#### 2. `grid-column` 属性和 `grid-row` 属性

`grid-column` 属性是 `grid-column-start` 、`grid-column-end` 这两个属性的简写。

`grid-row` 属性是 `grid-row-start` 、`grid-row-end` 这两个属性的简写。

语法结构如下所示：

```css
.item {
  grid-column: grid-column-start / grid-column-end;
  grid-row: grid-row-start / grid-row-end;
}
```

示例代码如下所示：

```css
.item1 {
  /* 1. 纯数字写法 */
  grid-column: 1 / 3;
  grid-row: 1 / 3;

  /* 2. 纯名字写法 */
  grid-column: c1 / c3;
  grid-row: r1 / r3;

  /* 3. 数字 + span 数字 */
  grid-column: 1 / span 2;
  grid-row: 1 / span 2;

  /* 4. 数字 + span 名字*/
  grid-column: 1 / span c3;
  grid-row: 1 / span r3;
}
```

### grid-area 属性

`grid-area` 属性用来指定网格项的区域，该属性可以理解为是 `grid-column-start` 、`grid-row-start` 、`grid-column-end` 、`grid-row-end` 这四个属性的简写。语法结构如下所示：

```css
.item {
  grid-area: <name> | <row-start> / <column-start> / <row-end> / <column-end> 
}
```

**属性值**：

- `name`: `grid-template-areas` 中定义的命名。
- `<row-start> / <column-start> / <row-end> / <column-end>`: 值为  `<number> | <name> | span <number> | span <name> | auto` 其中一种。

示例代码如下所示：

```css
.item1 {
  /* 
    第一个 1 表示 grid-row-start: 1
    第二个 1 表示 grid-columns-start: 1
    第一个 3 表示 grid-row-end: 3
    第二个 3 表示 grid-columns-end: 3
  */
  grid-area: 1 / 1 / 3 / 3;
}
```

效果图如下：

![image-20210509194719268](http://img.seecode.cc//picgo/image-20210509194719268.png)

### self

#### 1. `align-self` 属性

沿着列轴对齐网格项里面的内容，语法格式如下：

```css
.container {
  align-self: start | end | center | stretch;
}
```

#### 2. `justify-self` 属性

沿着**行轴**对齐网格项里面的内容，语法格式如下：

```css
.container {
  justify-self: start | end | center | stretch;
}
```

#### 2. `place-self` 属性

`place-self` 是一个简写属性，使用此属性可以同时设置**列轴**对齐和**行轴**对齐，语法结构如下：

```css
.container {
  place-self: start | end | center | stretch;
}
```

示例代码如下所示：

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta http-equiv="X-UA-Compatible" content="IE=edge" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>self 属性</title>
        <link rel="stylesheet" href="./init.css" />
        <style>
            .container {
                background-color: #fffae8;
                display: grid;
                width: 800px;
                height: 800px;
                margin: 0 auto;
                grid-template-columns: repeat(2, 400px);
                grid-template-rows: repeat(2, 400px);
            }
            .item {
                /* 所有item都居中 */
                place-self: center;
            }
            .item1 {
                /* item1水平靠右 垂直靠上 */
                justify-self: end;
                align-self: start;
            }
            .item2 {
                /* item2水平靠左 垂直靠下 */
                justify-self: start;
                align-self: end;
            }
        </style>
    </head>
    <body>
        <div class="container">
            <div class="item1 item">1</div>
            <div class="item2 item">2</div>
            <div class="item3 item">3</div>
            <div class="item4 item">4</div>
        </div>
    </body>
</html>
```

执行结果如下所示：

![image-20210509211320577](http://img.seecode.cc//picgo/image-20210509211320577.png)

(完)

> PS: 关于 Grid 布局有一个小游戏可以帮助我们练习 [Grid Garden - 一个用来学CSS grid的游戏](https://cssgridgarden.com/#zh-cn)