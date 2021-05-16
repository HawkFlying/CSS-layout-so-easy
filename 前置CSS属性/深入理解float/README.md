# 深入理解 float

[toc]

## float 的用法

### float 概述

`float` 的诞生之初并不是为了完成某种高级的布局，而是为了完成一个简单的文字环绕。

`float` 属性官方给的定义是指定某一个元素沿着其容器的左侧或右侧放置，允许文本和内联元素环绕它。为元素设置该元素后，元素会脱离文档流。

> 关于脱离文档流的详细说明，请见：[深入理解 position 定位属性]()

### float 属性值

`float` 的属性值主要有三个：

- `left`: 元素向左浮动

- `right`: 元素向右浮动

  > 开启浮动的元素称之为浮动元素

- `none`: 元素不进行浮动

示例代码如下

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta http-equiv="X-UA-Compatible" content="IE=edge" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>float属性值</title>
        <style>
            .container {
                width: 1640px;
                margin: 0 auto;
            }
            .content {
                width: 800px;
                background-color: #3498db;
                margin: 10px;
                float: left;
            }
            .item {
                height: 300px;
                width: 300px;
                background-color: #2ecc71;
                font-size: 100px;
                line-height: 300px;
                color: #eee;
            }
            p {
                margin: 5px;
            }
            /* 开启浮动 */
            .item-l {
                float: left;
            }
            .item-r {
                float: right;
            }
        </style>
    </head>
    <body>
        <div class="container">
            <div class="content">
                <div class="item item-l">左浮动</div>
                <p>我如果爱你——绝不像攀援的凌霄花，借你的高枝炫耀自己；</p>
                ...
            </div>
            <div class="content">
                <div class="item item-r">右浮动</div>
                <p>我如果爱你——绝不像攀援的凌霄花，借你的高枝炫耀自己；</p>
                ...
            </div>
        </div>
    </body>
</html>

```

执行结果如下图所示:

![image-20210516122525498](http://img.seecode.cc//picgo/image-20210516122525498.png)

### 基本特性

开启 `float` 属性的元素会具有两个基本特性：

1. 包裹性：所谓的包裹性就是指元素的宽度会收缩到与内容一致。
2. 破坏性：所谓的破坏性指的就是父元素的高度塌陷

## 清除浮动

### 1. 使用带 clear 属性的空元素

在浮动元素后使用一个空元素如 `<div class="clear"></div>` ，并在CSS中赋予 `.clear{clear:both;}` 属性即可清理浮动。亦可使用 `<br class="clear" /> 或<hr class="clear" />` 来进行清理。

示例代码如下：

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta http-equiv="X-UA-Compatible" content="IE=edge" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>使用带 clear 属性的空元素</title>
        <style>
            .news {
                background-color: #ff4757;
                border: solid 1px #eccc68;
                width: 1000px;
                margin: 0 auto;
            }

            .news img {
                float: left;
                width: 700px;
            }

            .news h1 {
                float: right;
            }
            /* 清除浮动元素 */
            .clear {
                clear: both;
            }
        </style>
    </head>
    <body>
        <div class="news">
            <img src="../image/img.jpg" />
            <h1>some text</h1>
            <div class="clear"></div>
        </div>
    </body>
</html>
```

> 优点：简单，代码少，浏览器兼容性好。
> 缺点：需要添加大量无语义的 HTML 元素，代码不够优雅，后期不容易维护。

### 3. 使用 CSS 的 overflow 属性

为浮动元素的容器元素添加 `overflow:hidden; `或 `overflow:auto;` 可以达到清除浮动的效果

示例代码如下：

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta http-equiv="X-UA-Compatible" content="IE=edge" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>使用带 clear 属性的空元素</title>
        <style>
            .news {
                background-color: #ff4757;
                border: solid 1px #eccc68;
                width: 1000px;
                margin: 0 auto;
                /* 通过添加 overflow: hidden; 实现清除浮动效果 */
                overflow: hidden;
            }

            .news img {
                float: left;
                width: 700px;
            }

            .news h1 {
                float: right;
            }
        </style>
    </head>
    <body>
        <div class="news">
            <img src="../image/img.jpg" />
            <h1>some text</h1>
            <div class="clear"></div>
        </div>
    </body>
</html>
```

> 优点：简单，代码少，浏览器支持好
>
> 缺点：不适用于高度固定的盒子，内容超出时会被隐藏

### 3. 使用 CSS 的 `:after` 伪元素

示例代码如下：

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta http-equiv="X-UA-Compatible" content="IE=edge" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>使用 CSS 的 `:after` 伪元素</title>
        <style>
            .news {
                background-color: #ff4757;
                border: solid 1px #eccc68;
                width: 1000px;
                margin: 0 auto;
            }

            .news img {
                float: left;
                width: 700px;
            }

            .news h1 {
                float: right;
            }
            /* 为父容器添加一个 class  */
            .clearfix:after {
                content: '';
                display: block;
                height: 0;
                clear: both;
                visibility: hidden;
            }
        </style>
    </head>
    <body>
        <div class="news clearfix">
            <img src="../image/img.jpg" />
            <h1>some text</h1>
        </div>
    </body>
</html>
```

> 优点：浏览器支持好，不容易出现怪问题。
>
> 缺点：代码多
>
> 建议：推荐使用，建议定义公共类，以减少CSS代码