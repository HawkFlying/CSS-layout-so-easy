# Flex 布局

> **古之立大事者，不惟有超世之才，亦必有坚韧不拔之志** —— 苏轼

[toc]

## 写在前面

### 弹性盒子模型是什么

CSS3新增了弹性盒子模型( *Flexible Box*或*FlexBox*)，是一种新的用于在HTML页面实现布局的方式。使得当HTML页面适应不同尺寸的屏幕和不同的设备时，元素是可预测地运行。

弹性盒子模型实现HTML页面布局是与方向无关的。不类似于块级布局侧重垂直方向，内联布局侧重水平方向。

弹性盒子模型主要**适用于HTML页面的组件以及小规模的布局**，而并**不适用于大规模的布局**，**否则会影响 HTML 页面性能**。

### 弹性盒子模型相关概念

CSS3新增的弹性盒子模型是一个完整的模块，涉及的样式属性较多。首先，对弹性盒子模型的相关概念完成基本的了解。

![弹性盒子模型图解.png (1233×432) (wangshmily.top)](http://img.seecode.cc//picgo/%E5%BC%B9%E6%80%A7%E7%9B%92%E5%AD%90%E6%A8%A1%E5%9E%8B%E5%9B%BE%E8%A7%A3.png)

- 伸缩容器(*flex container*) :包裹伸缩项目的父元素。
- 伸缩项目(*flex item*) :伸缩容器的每个子元素。
- 轴(*axis*) :每个弹性盒子模型拥有两个轴。
  - 主轴(*main axis*) :伸缩项目沿其一次排列的轴被称为主轴。
  - 侧轴(*cross axis*) :垂直于主轴的轴被称为侧轴。
  - 方向(*direction*) :伸缩容器的主轴由主轴起点和主轴终点，侧轴由侧轴起点和侧轴终点描述伸缩项目排列的方向。
- 尺寸(*dimension*) :根据伸缩容器的主轴和侧轴，伸缩项目的宽度和高度。
  - 对应主轴的称为主轴尺寸。
  - 对应侧轴的称为侧轴尺寸。

### 定义弹性盒子模型

CSS3中想要设置为弹性盒子模型的话，需要通过 `display` 样式属性设置值为 `flex` 或 `inline-flex` 即可。

```css
display: fiex; /* 值 flex 使弹性容器成为块级元素。 */
/* 或者 */
display: inline-fiex; /* 值 inline-flex 使弹性容器成为单个不可分的行内级元素。 */
```

> 以上代码就可以指定某个元素为弹性盒子模型，该元素成为伸缩容器，子元素则成为伸缩项目。
>

弹性盒子模型也存在浏览器兼容问题，解决方案如下：

```css
/* WebKit引擎的浏览器(Chrome, Safari, Opera) */
display: -webkit-fiex;
/* Trident引擎的浏览器(IE 10+) */
dispaly: -ms-fiex;
/* Gecko引擎的浏览器(Firefox) */
display: -moz-fiex;
/* Presto引擎的浏览器(Opera) */
dispaly: -o-fiex;
```

示例代码如下：定义一个简单的 Flex Box

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>定义弹性盒子模型</title>
        <link rel="stylesheet" href="./init.css" />
        <style>
            /* 
                将父级元素设置为弹性盒子模型
                * 这样设置的话子级所有元素都为弹性盒子模型
                * 默认情况下，所有子元素作为伸缩项目都是沿着主轴水平排列
            */
            .container {
                display: flex;
            }
        </style>
    </head>

    <body>
        <!-- HTML 结构为父子级结构 -->
        <div class="container">
            <div class="item1 item">1</div>
            <div class="item2 item">2</div>
            <div class="item3 item">3</div>
        </div>
    </body>
</html>

```

> 这里并没有开启浮动之类的关于布局的样式，仅仅将 `.container` 的 `display` 设置为 `flex`

执行结果如下所示；

![image-20210509222425319](http://img.seecode.cc//picgo/image-20210509222425319.png)

>   默认情况下，所有子元素作为伸缩项目都是沿着主轴水平排列


## 伸缩容器的属性

### flex-direction 属性

CSS `flex-direction` 属性指定了内部元素是如何在 flex 容器中布局的，定义了主轴的方向(正方向或反方向)。

语法结构

```css
flex-direction: row | row-reverse | column | column-reverse;
```

值

- row： 默认值，flex容器的主轴被定义为与文本方向相同。 主轴起点和主轴终点与内容方向相同(起点在左端)。

- row-reverse： 表现和row相同，但是置换了主轴起点和主轴终点(起点在右端)

- column： flex容器的主轴和块轴相同。主轴起点与主轴终点和书写模式的前后点相同(起点在上沿)

- column-reverse： 表现和column相同，但是置换了主轴起点和主轴终点明(起点在下沿)

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta http-equiv="X-UA-Compatible" content="IE=edge" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>02 flex-direction属性</title>
        <link rel="stylesheet" href="./init.css" />
        <style>
            body {
                display: flex;
            }
            .container {
                display: flex;
                margin: 20px;
            }
            /* 
                flex-direction 属性
                * 作用：创建主轴方向
                * 适用于伸缩容器元素
                * 值
                    * row: 设置主轴是水平方向 默认
                    * row-reverse: 与 row 的方向相反
                    * column：设置主轴的方向是垂直方向
                    * column-reverse: 与column 方向相反
            */
            .row {
                /* 默认，水平排列 */
                flex-direction: row;
                height: 200px;
            }
            .row-reverse {
                /* 水平排列，反向 */
                flex-direction: row-reverse;
                height: 200px;
            }
            .column {
                /* 垂直排列 */
                flex-direction: column;
            }
            .column-reverse {
                /* 垂直排列 反向 */
                flex-direction: column-reverse;
            }
        </style>
    </head>
    <body>
        <div class="container row">
            <div class="item1 item">1</div>
            <div class="item2 item">2</div>
            <div class="item3 item">3</div>
        </div>
        <div class="container row-reverse">
            <div class="item1 item">1</div>
            <div class="item2 item">2</div>
            <div class="item3 item">3</div>
        </div>
        <div class="container column">
            <div class="item1 item">1</div>
            <div class="item2 item">2</div>
            <div class="item3 item">3</div>
        </div>
        <div class="container column-reverse">
            <div class="item1 item">1</div>
            <div class="item2 item">2</div>
            <div class="item3 item">3</div>
        </div>
    </body>
</html>

```

执行效果如下

![image-20210509224157740](http://img.seecode.cc//picgo/image-20210509224157740.png)

### justify-content属性

CSS justify-content 属性适用于伸缩容器元素，用于设置伸缩项目沿着**主轴线的对齐方式**。

语法结构

```css
justify-content: center| flex-start| flex-end| space-between| space-around;
```

值

- center: 伸缩项目向第-行的中间位置对齐。
- flex-start: 伸缩项目向第一行的开始位 置对齐。
- flex-end: 伸缩项目向第一行的结束位置对齐。
- space-between: 伸缩项目会平均分布在一行中。
- space-around: 伸缩项目会平均分布在一行中， 两端保留一半的空间。 

示例代码

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>justify-content属性</title>
  <style>
    .box {
      width: 1100px;
      margin: 0 auto;
    }

    .container {
      height: 100px;
      width: 1000px;
      border: 1px solid black;
      margin-bottom: 10px;

      /* 开启弹性盒子模型 */
      display: flex;
    }

    .container div {
      width: 200px;
      height: 100px;
      font-size: 66px;
      text-align: center;
      line-height: 100px;
    }

    .container div:nth-child(1) {
      background-color: blue;
    }

    .container div:nth-child(2) {
      background-color: red;
    }

    .container div:nth-child(3) {
      background-color: green;
    }

    /* 
      justify-content属性
      * 作用：设置伸缩项目沿着主轴线的对齐方式
      * 适用于伸缩容器元素
      * 值
        * center: 伸缩项目向第-行的中间位置对齐。
        * flex-start: 伸缩项目向第一行的开始位 置对齐。
        * flex-end: 伸缩项目向第一行的结束位置对齐。
        * space-between: 伸缩项目会平均分布在一行中。
        * space-around: 伸缩项目会平均分布在一行中， 两端保留一半的空间。 
     */

    /* 伸缩项目向第-行的中间位置对齐 */
    .c1 {
      justify-content: center;
    }

    /* 伸缩项目向第一行的开始位 置对齐。 */
    .c2 {
      justify-content: flex-start;
    }

    /* 伸缩项目向第一行的结束位置对齐 */
    .c3 {
      justify-content: flex-end;
    }

    /*  伸缩项目会平均分布在一行中 */
    .c4 {
      justify-content: space-between;
    }

    /* 伸缩项目会平均分布在一行中， 两端保留一半的空间 */
    .c5 {
      justify-content: space-around;
    }
  </style>
</head>

<body>
  <div class="box">
    <div class="container c1">
      <div>c1</div>
      <div>c1</div>
      <div>c1</div>
    </div>
    <div class="container c2">
      <div>c2</div>
      <div>c2</div>
      <div>c2</div>
    </div>
    <div class="container c3">
      <div>c3</div>
      <div>c3</div>
      <div>c3</div>
    </div>
    <div class="container c4">
      <div>c4</div>
      <div>c4</div>
      <div>c4</div>
    </div>
    <div class="container c5">
      <div>c5</div>
      <div>c5</div>
      <div>c5</div>
    </div>
  </div>
</body>

</html>
```

执行效果如下

![image-20200708012633221](http://img.wangshmily.top/image-20200708012633221.png)

> **注意**： 实现的是伸缩项目相对于伸缩容器的对齐方式，与页面无关

### align-items属性

CSS `align-items` 属性适用于伸缩容器元素，用于设置伸缩项目所在行沿着**侧轴线的对齐方式**。

语法结构

```css
align-items: center | flex-start| flex-end| baseline| stretch;
```

值

- center: 伸缩项目向侧轴的中间位置对齐。
- flex-start: 伸缩项目向侧轴的起点位置对齐。
- flex- end: 伸缩项目向侧轴的终点位置对齐。
- baseline: 伸缩项目根据伸缩项目的基线对齐。
- stretch: 默认值，伸缩项目拉伸填充整个伸缩容器。

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>align-items属性</title>
  <style>
    .box {
      width: 1300px;
      margin: 0 auto;
    }

    .container {
      height: 600px;
      width: 200px;
      float: left;
      border: 1px solid black;
      margin-left: 20px;
      margin-bottom: 10px;

      /* 开启弹性盒子模型 */
      display: flex;
      /* 垂直布局 */
      flex-direction: column;
    }

    .container div {
      width: 50px;
      height: 150px;
      font-size: 66px;
      text-align: center;
      line-height: 150px;
    }

    .container div:nth-child(1) {
      background-color: blue;
    }

    .container div:nth-child(2) {
      background-color: red;
    }

    .container div:nth-child(3) {
      background-color: green;
    }

    /* 
      align-items属性
      * 作用：设置伸缩项目所在行沿着侧轴线的对齐方式
      * 适用于伸缩容器元素
      * 值
        - center: 伸缩项目向侧轴的中间位置对齐。
        - flex-start: 伸缩项目向侧轴的起点位置对齐。
        - flex-end: 伸缩项目向侧轴的终点位置对齐。
        - baseline: 伸缩项目根据伸缩项目的基线对齐。
        - stretch: 默认值，伸缩项目拉伸填充整个伸缩容器。
     */




    /* 伸缩项目向侧轴的终点位置对齐 */
    .c1 {
      align-items: flex-end;
    }

    /* 伸缩项目向侧轴的中间位置对齐 */
    .c2 {
      align-items: center;
    }


    /* 伸缩项目向侧轴的起点位置对齐。 */
    .c3 {
      align-items: flex-start;
    }

    /*  伸缩项目根据伸缩项目的基线对齐 */
    .c4 {
      align-items: baseline;
    }

    /* 默认值，伸缩项目拉伸填充整个伸缩容器 */
    .c5 {
      align-items: stretch;
    }
  </style>
</head>

<body>
  <div class="box">
    <div class="container c1">
      <div>c1</div>
      <div>c1</div>
      <div>c1</div>
    </div>
    <div class="container c2">
      <div>c2</div>
      <div>c2</div>
      <div>c2</div>
    </div>
    <div class="container c3">
      <div>c3</div>
      <div>c3</div>
      <div>c3</div>
    </div>
    <div class="container c4">
      <div>c4</div>
      <div>c4</div>
      <div>c4</div>
    </div>
    <div class="container c5">
      <div>c5</div>
      <div>c5</div>
      <div>c5</div>
    </div>
  </div>
</body>

</html>
```

执行效果如下

![image-20200708014005578](http://img.wangshmily.top/image-20200708014005578.png)

> 配合 `justify-content` 属性，可以做出水平垂直居中

### flex-wrap属性

CSS `flex-wrap` 属性适用于伸缩容器元素，用于**设置**伸缩容器的**子元素是单行显示还是多行显示**。

语法结构

```css
flex-wrap: nowrap| wrap| wrap-reverse
```

值

- `nowrap`:设置伸缩项目单行显示。这种方式可能导致溢出伸缩容器
- `wrap`:设置伸缩项目多行显示。
- `wrap-reverse`:与wrap相反。

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>justify-content属性</title>
  <style>
    .box {
      width: 1100px;
      margin: 0 auto;
    }

    .container {
      height: 400px;
      width: 800px;
      border: 1px solid black;
      margin-bottom: 10px;

      /* 开启弹性盒子模型 */
      display: flex;
    }

    .container div {
      width: 400px;
      height: 100px;
      font-size: 66px;
      text-align: center;
      line-height: 100px;
    }

    .container div:nth-child(1) {
      background-color: blue;
    }

    .container div:nth-child(2) {
      background-color: red;
    }

    .container div:nth-child(3) {
      background-color: green;
    }

    /* 
      flex-wrap属性
      * 作用：设置伸缩容器的子元素是单行显示还是多行显示。
      * 适用于伸缩容器元素
      * 值
        - nowrap:设置伸缩项目单行显示。这种方式可能导致溢出伸缩容器
        - wrap:设置伸缩项目多行显示。
        - wrap-reverse:与wrap相反。

     */

    /* 设置伸缩项目单行显示 */
    .c1 {
      flex-wrap: nowrap;
    }

    /* 设置伸缩项目多行显示。 */
    .c2 {
      flex-wrap: wrap;
    }

    /* 设置为wrap-reverse */
    .c3 {
      flex-wrap: wrap-reverse;
    }
  </style>
</head>

<body>
  <div class="box">
    <div class="container c1">
      <div>c1</div>
      <div>c1</div>
      <div>c1</div>
    </div>
    <div class="container c2">
      <div>c2</div>
      <div>c2</div>
      <div>c2</div>
    </div>
    <div class="container c3">
      <div>c3</div>
      <div>c3</div>
      <div>c3</div>
    </div>
  </div>
  </div>
</body>

</html>
```

执行效果如下

![image-20200708015137935](http://img.wangshmily.top/image-20200708015137935.png)

> 如果设置伸缩容器的宽度小于所有子元素宽度之和的话，子元素并没有自动换行也没有溢出。
>
> 效果果根据伸缩容器的宽度自动调整所有子元素的宽度

### align-content属性

CSS `align-content` 属性适用于伸缩容器元素，用于设置**伸缩行**的对齐方式。该属性会更改 `flex-wrap` 属性的效果。

**该属性对单行弹性盒子模型无效。**

语法结构

```css
align-content: center| flex-start| flex-end| space-between| space-around| stretch;
```

值

- center: 各行向伸缩容器的中间位置对齐。
- flex-start: 各行向伸缩容器的起点位置对齐。
- flex-end: 各行向伸缩容器的终点位置对齐。
- space-between: 各行会平均分布在一行中。
- space-around: 各行会平均分布在一行中， 两端保留-半的空间。
- stretch: 默认值，各行将会伸展以占用额外的空间。

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>align-content属性</title>
  <style>
    .box {
      width: 1300px;
      margin: 0 auto;
    }

    .container {
      height: 600px;
      width: 200px;
      float: left;
      border: 1px solid black;
      margin-left: 20px;
      margin-bottom: 10px;

      /* 开启弹性盒子模型 */
      display: flex;
      /* 多行显示 */
      flex-wrap: wrap;
    }

    .container div {
      width: 150px;
      height: 150px;
      font-size: 66px;
      text-align: center;
      line-height: 150px;
    }

    .container div:nth-child(1) {
      background-color: blue;
    }

    .container div:nth-child(2) {
      background-color: red;
    }

    .container div:nth-child(3) {
      background-color: green;
    }
    /* 
      align-content属性
      * 作用：设置设置伸缩行的对齐方式
      * 适用于伸缩容器元素
      * 值
        - center: 各行向伸缩容器的中间位置对齐。
        - flex-start: 各行向伸缩容器的起点位置对齐。
        - flex-end: 各行向伸缩容器的终点位置对齐。
        - space-between: 各行会平均分布在一行中。
        - space-around: 各行会平均分布在一行中， 两端保留-半的空间。
        - stretch: 默认值，各行将会伸展以占用额外的空间。
      * 注意：该属性对单行无效，即：带有 flex-wrap: nowrap
     */
    /* 各行向伸缩容器的终点位置对齐 */
    .c1 {
      align-content: flex-end;
    }

    /* 各行向伸缩容器的中间位置对齐 */
    .c2 {
      align-content: center;
    }


    /* 各行向伸缩容器的起点位置对齐。 */
    .c3 {
      align-content: flex-start;
    }

    /*  各行会平均分布在一行中 */
    .c4 {
      align-content: space-between;
    }

    /* 各行会平均分布在一行中， 两端保留-半的空间 */
    .c5 {
      align-content: space-around;
    }
  </style>
</head>

<body>
  <div class="box">
    <div class="container c1">
      <div>c1</div>
      <div>c1</div>
      <div>c1</div>
    </div>
    <div class="container c2">
      <div>c2</div>
      <div>c2</div>
      <div>c2</div>
    </div>
    <div class="container c3">
      <div>c3</div>
      <div>c3</div>
      <div>c3</div>
    </div>
    <div class="container c4">
      <div>c4</div>
      <div>c4</div>
      <div>c4</div>
    </div>
    <div class="container c5">
      <div>c5</div>
      <div>c5</div>
      <div>c5</div>
    </div>
  </div>
</body>

</html>
```

执行效果如下

![image-20200708021034222](http://img.wangshmily.top/image-20200708021034222.png)

> **注意**： 该属性对单行弹性盒子模型无效，即：带有 flex-wrap: nowrap

### flex-flow属性

CSS flex-flow属性适用于伸缩容器元素，该属性是`flex-direction`和`flex-wrap`的简写。

语法结构

```css
flex-flow: <'flex-direction'> || <'flex-wrap'>
```

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>flex-flow属性</title>
  <style>
    .box {
      width: 1100px;
      margin: 0 auto;
    }

    .container {
      height: 300px;
      width: 1000px;
      border: 1px solid black;
      margin-bottom: 5px;

      /* 开启弹性盒子模型 */
      display: flex;
    }

    .container div {
      width: 400px;
      height: 150px;
      font-size: 66px;
      text-align: center;
      line-height: 150px;
    }

    .container div:nth-child(1) {
      background-color: blue;
    }

    .container div:nth-child(2) {
      background-color: red;
    }

    .container div:nth-child(3) {
      background-color: green;
    }

    /* 
      flex-flow 属性
      * 作用：是 flex-direction 属性和 flex-warp 属性的缩写
     */
    .c1 {
      /* 默认，水平排列 换行*/
      flex-flow: row wrap;
    }

    .c2 {
      /* 水平排列，反向 换行*/
      flex-flow: row-reverse wrap;
    }
  </style>
</head>

<body>
  <div class="box">
    <div class="container c1">
      <div>c1</div>
      <div>c1</div>
      <div>c1</div>
    </div>
    <div class="container c2">
      <div>c2</div>
      <div>c2</div>
      <div>c2</div>
    </div>
  </div>
  </div>
</body>

</html>
```

执行效果如下

![image-20200709011719825](http://img.wangshmily.top/image-20200709011719825.png)

## 伸缩项的属性

### flex属性

CSS `flex` 属性是一个简写属性，用于设置伸缩项目如何伸长或缩短以适应伸缩容器中的可用空间。

语法结构

```css
flex: auto | initial | none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
```

- none: 元素会根据自身宽高来设置尺寸。它是完全非弹性的：既不会缩短，也不会伸长来适应 flex 容器。相当于将属性设置为"`flex: 0 0 auto`"。
- auto: 元素会根据自身的宽度与高度来确定尺寸，但是会伸长并吸收 flex 容器中额外的自由空间，也会缩短自身来适应 flex 容器。这相当于将属性设置为 "`flex: 1 1 auto`".

当前属性可以拆分为

- flex-grow：设置了一个flex项主尺寸的flex增长系数。它指定了伸缩容器中剩余空间的多少应该分配给项目，值默认为0

- flex-shrink：指定了 flex 元素的收缩规则。flex 元素仅在默认宽度之和大于容器的时候才会发生收缩

- flex-basis：指定了 flex 元素在主轴方向上的初始大小。

`flex` 属性可以指定1个，2个或3个值。

**单值语法**: 值必须为以下其中之一:

- 一个无单位数(`<number>`): 它会被当作 `<flex-grow>` 的值。
- 一个有效的宽度(width)值: 它会被当作 `<flex-basis>`的值。
- 关键字`none`，`auto`或`initial`.

**双值语法**: 第一个值必须为一个无单位数，并且它会被当作 `<flex-grow>` 的值。第二个值必须为以下之一：

- 一个无单位数：它会被当作 `<flex-shrink>` 的值。
- 一个有效的宽度值: 它会被当作 `<flex-basis>` 的值。

**三值语法**:

- 第一个值必须为一个无单位数，并且它会被当作 `<flex-grow>` 的值。
- 第二个值必须为一个无单位数，并且它会被当作  `<flex-shrink>` 的值。
- 第三个值必须为一个有效的宽度值， 并且它会被当作 `<flex-basis>` 的值。

测试关键字

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>flow属性</title>
  <style>
    .container {
      height: 150px;
      border: 1px solid black;
      margin-bottom: 5px;

      /* 开启弹性盒子模型 */
      display: flex;
    }

    .container div {
      width: 200px;
      height: 100px;
      font-size: 66px;
      text-align: center;
      line-height: 100px;
    }

    .container div:nth-child(1) {
      background-color: blue;
    }

    .container div:nth-child(2) {
      background-color: red;
    }

    .container div:nth-child(3) {
      background-color: green;
    }

    .c1 div:nth-child(1) {
      /* 
        当将其设置为none时，为其设置多大的盒子就是多个的盒子，不会自动变更
      */
      flex: none;
    }

    .c1 div:nth-child(2) {
      /* 当为auto时，自动适应至容器大小 */
      flex: auto
    }

    .c1 div:nth-child(3) {
      flex: none;
    }
  </style>
</head>

<body>
  <div class="box">
    <div class="container c1">
      <div>c1</div>
      <div>c1</div>
      <div>c1</div>
    </div>
  </div>
  </div>
</body>

</html>
```

执行效果如下

![image-20200709021634224](http://img.wangshmily.top/image-20200709021634224.png)

> 使用这种方式就简单做出一个 定宽+自适应+定宽的三列布局

完成等分布局

```css
.container {
  height: 150px;
  border: 1px solid black;
  margin-bottom: 5px;

  /* 开启弹性盒子模型 */
  display: flex;
}

.container div {
  width: 200px;
  height: 100px;
  font-size: 66px;
  text-align: center;
  line-height: 100px;

  /* 使用<flex-grow>可以做出等分布局 */
  flex: 1;
}

.container div:nth-child(1) {
  background-color: blue;
}

.container div:nth-child(2) {
  background-color: red;
}

.container div:nth-child(3) {
  background-color: green;
}
```

执行效果如下

![image-20200709022459185](http://img.wangshmily.top/image-20200709022459185.png)

### align-self属性

CSS align-self属性适用于伸缩容器元素,于设置伸缩项目自身元素在侧轴的对齐方式。

语法结构

```css
align-self: center| flex-start| flex-end| baseline| stretch;
```

- center:伸缩项目向侧轴的中间位置对齐。
- flex- start:伸缩项目向侧轴的起点位置对齐。
- flex-end:伸缩项目向侧轴的终点位置对齐。
- baseline:伸缩项目根据伸缩项目的基线对齐。
- stretch:默认值，伸缩项目拉伸填充整个伸缩容器。

示例代码

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>align-self属性</title>
  <style>
    .container {
      height: 300px;
      width: 800px;
      float: left;
      border: 1px solid black;
      margin-left: 20px;
      margin-bottom: 10px;

      /* 开启弹性盒子模型 */
      display: flex;
    }

    .container div {
      width: 200px;
      height: 50px;
    }

    .container div:nth-child(1) {
      background-color: blue;
    }

    .container div:nth-child(2) {
      background-color: red;
      /* 
        align-self属性
        * 作用：设置伸缩项目自身元素在侧轴的对齐方式
        * 值
          - center:伸缩项目向侧轴的中间位置对齐。
          - flex- start:伸缩项目向侧轴的起点位置对齐。
          - flex-end:伸缩项目向侧轴的终点位置对齐。
          - baseline:伸缩项目根据伸缩项目的基线对齐。
          - stretch:默认值，伸缩项目拉伸填充整个伸缩容器。
       */
      align-self: center;
    }

    .container div:nth-child(3) {
      background-color: green;
      /* 伸缩项目沿底部对齐 */
      align-self: flex-end;
    }
  </style>
</head>

<body>
  <div class="box">
    <div class="container">
      <div></div>
      <div></div>
      <div></div>
    </div>
  </div>
  </div>
</body>

</html>
```

执行效果如下

![image-20200709024252428](http://img.wangshmily.top/image-20200709024252428.png)

### order 属性

CSS `order` 属性规定了弹性容器中的可伸缩项目在布局时的顺序。元素按照 `order` 属性的值的增序进行布局。拥有相同 `order` 属性值的元素按照它们在源代码中出现的顺序进行布局。

语法结构

```css
order: <integer>
```

`<integer>`: 表示此可伸缩项目所在的次序组。

> **注意**: `order` 仅仅对元素的视觉顺序产生作用，并不会影响元素的逻辑顺序。

示例代码

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>align-self属性</title>
  <style>
    .container {
      height: 200px;
      border: 1px solid black;
      margin-left: 20px;
      margin-bottom: 10px;

      /* 开启弹性盒子模型 */
      display: flex;
    }

    .container div {
      width: 300px;
      height: 200px;
    }

    .container div:nth-child(1) {
      background-color: blue;
    }

    .container div:nth-child(2) {
      background-color: red;
    }

    .container div:nth-child(3) {
      background-color: green;
    }

    /* 
      order属性用于改变其伸缩项目的顺序
     */
    .c2>div:nth-child(1) {
      order: 2;
    }

    .c2>div:nth-child(2) {
      order: 1;
    }

    .c2>div:nth-child(3) {
      order: 3;
    }
  </style>
</head>

<body>
  <div class="box">
    <div class="container c1">
      <div></div>
      <div></div>
      <div></div>
    </div>
    <div class="container c2">
      <div></div>
      <div></div>
      <div></div>
    </div>
  </div>
  </div>
</body>

</html>
```

执行效果如下

![image-20200709025328138](http://img.wangshmily.top/image-20200709025328138.png)

> 使用这个属性可以非常简单的改变其布局



## 用到的 CSS 样式

```css
body {
    margin: 0;
    padding: 20px;
}
.container {
    background-color: #fffae8;
}
.item {
    height: 200px;
    width: 200px;
    line-height: 200px;
    text-align: center;
    font-size: 80px;
}
.item1 {
    background-color: #e6005c;
}
.item2 {
    background-color: #777bce;
}
.item3 {
    background-color: #c9780c;
}
.item4 {
    background-color: #fef3c9;
}
.item5 {
    background-color: #ce3b3b;
}
.item6 {
    background-color: #e666ff;
}
.item7 {
    background-color: #f4ea20;
}
.item8 {
    background-color: #b4a4ca;
}

```

