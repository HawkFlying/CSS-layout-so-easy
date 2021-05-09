# Grid 布局

> **古之立大事者，不惟有超世之才，亦必有坚韧不拔之志** —— 苏轼

## 写在前面

Grid 布局(*网格布局*)是 CSS 最新的也是最强大的一种布局方案。

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

#### `grid-template-columns` 属性 和 `grid-template-rows` 属性

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

#### 网格线的名称

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

#### `grid-template-areas` 属性

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

#### 	

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

`gap` 属性是 `column-gap` 和 `row-gap` 属性的简写。

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



> 值得注意的是 `gap` 、`column-gap` 和 `row-gap`  属性在之前是有 `grid-` 的前缀的，即 `grid-gap` 、`grid-column-gap` 和 `grid-row-gap`。在使用的过程中，为了保证有效，我们可以这两个属性一起写。



### items 属性

### content 属性

### grid-auto 属性

### grid 属性