# 深入理解 position 定位属性

[toc]

## 写在前面

### `position` 属性概述

`position` 属性是掌握 CSS 布局中重要得一个属性，该属性用于指定一个元素在文档中的定位方式。通过 `top`，right，`bottom` 和 `left` 属性则决定了该元素的最终位置。

该属性具有以下五个值：

- `static`: 默认值，表示正常布局行为，此时设置  `top`, `right`, `bottom`, `left` 和 `z-index ` 属性均无效。
- `relative`： **将元素设置为相对定位元素**，该方式不脱离文档流
- `absolute`：  **将元素设置为绝对定位元素**，使元素相对于最近的非 `static` 定位祖先元素的进行定位。
- `fixed`: **将元素设置为绝对定位元素**，使元素相对于视觉窗口进行定位。
- `sticky`: **将元素设置为粘性定位元素**，一开始不脱离文档流在默认位置，当到达某个位置时，相对于视口进行定位。

## absolute 属性值

### 基本特性

`absolute` 属性值与 `float` 属性名，具有相同的特性：

1. 包裹性：所谓的包裹性就是指元素的宽度会收缩到与内容一致。
2. 破坏性：所谓的破坏性指的就是父元素的高度探险

### 脱离文档流

将 `position` 属性的值设置为 `absolute` 时，其元素会脱离文档流。

文档流就是将窗体自上而下分成一行一行，并在每行中按从左至右依次排放元素，称为文档流，也称为普通流。

所谓的脱离文档流就是元素不再在文档流中占据空间，而是处于浮动状态（可以理解为漂浮在文档流的上方）。脱离文档流的元素的定位基于正常的文档流，当一个元素脱离文档流后，依然在文档流中的其他元素将忽略该元素并填补其原先的空间。

示例代码如下：

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta http-equiv="X-UA-Compatible" content="IE=edge" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>脱离文档流</title>
        <style>
            .item {
                width: 300px;
                height: 200px;
                background-color: #ec407a;
                /* 脱离文档流 不占文档中的空间 */
                position: absolute;
            }
            img {
                width: 500px;
            }
        </style>
    </head>
    <body>
        <div class="item"></div>
        <img src="./../image/img.jpg" />
    </body>
</html>
```

不脱离文档流的表现如下图所示：

![image-20210514205208841](http://img.seecode.cc//picgo/image-20210514205208841.png)

脱离文档流的表现如下图所示：

![image-20210514205238054](http://img.seecode.cc//picgo/image-20210514205238054.png)

可以看出 `<div>` 元素已经漂浮在图片的上方了。

### 与 `margin` 共同使用

`absolute` 属性值与 `margin` 共同使用时可以完成一些效果很不错的布局。例如一个搜索框效果，示例代码如下所示：

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <title>下拉框定位二三事</title>
        <style>
            body {
                margin: 0;
                background-color: #edeff0;
            }
            /* 容器样式 */
            .container {
                margin-top: 120px;
                margin-left: 240px;
                overflow: hidden;
            }
            /* 输入框样式 */
            .input {
                width: 240px;
                line-height: 18px;
                padding: 10px;
                margin: 0;
                border: 0 none;
            }
            .input:focus {
                outline: 0 none;
            }

            .list {
                /* 默认不显示，当输入内容时，通过js控制其显示 */
                /* display: none; */
                position: absolute;
                width: 260px;
                /* 通过 margin 来控制其显示的位置 */
                margin: 39px 0 0 -1px;
                padding-left: 0;
                list-style-type: none;
                border: 1px solid #e6e8e9;
                background-color: #fff;
                box-shadow: 0px 1px 2px #d5d7d8;
                font-size: 12px;
            }
            /* 列表项样式以及悬停样式 */
            .list > li {
                line-height: 30px;
                padding-left: 12px;
            }
            .list > li:hover {
                background-color: #f9f9f9;
            }
            .list a {
                display: block;
                color: #5e5e5e;
                text-decoration: none;
            }
            .list a:hover {
                color: #000;
            }
        </style>
    </head>

    <body>
        <div class="container">
            <ul class="list">
                <li>
                    <a>玩转CSS布局之 Grid 布局</a>
                </li>
                <li>
                    <a>玩转CSS布局之 Flex 布局</a>
                </li>
                <li>
                    <a>玩转CSS布局之深入理解 position 定位</a>
                </li>
                <li>
                    <a>玩转CSS布局之深入理解 z-index 定位</a>
                </li>
            </ul>
            <input class="input" placeholder="请输入内容" />
        </div>
    </body>
</html>

```

执行结果如下图所示：

![image-20210514221831036](http://img.seecode.cc//picgo/image-20210514221831036.png)

此时可以通过 `JavaScript` 的方式来控制提示内容的显示与隐藏。

### left、right、top、bottom 的使用

开启定位之后，可以通过这四个属性来控制其偏移量，其参数可以传递像素值，百分比(代表元素包含块的宽度的百分比)等。

简单的使用这里就不介绍了，这里介绍一些在开发中的小技巧

1. 没有宽度和高度声明实现的全屏自适应效果

   示例代码如下所示：

   ```html
   <!DOCTYPE html>
   <html lang="en">
       <head>
           <meta charset="UTF-8" />
           <meta http-equiv="X-UA-Compatible" content="IE=edge" />
           <meta name="viewport" content="width=device-width, initial-scale=1.0" />
           <title>没有宽度和高度声明实现的全屏自适应效果</title>
           <style>
               .overlay {
                   position: absolute;
                   /* 将其元素拉满整个页面 */
                   left: 0;
                   top: 0;
                   right: 0;
                   bottom: 0;
                   background-color: #000;
                   opacity: 0.5;
               }
           </style>
       </head>
   
       <body>
           <div class="overlay"></div>
       </body>
   </html>
   ```

2. `left` `right` 和 `width` 实现水平居中，示例代码如下所示：

   ```html
   <!DOCTYPE html>
   <html lang="en">
       <head>
           <meta charset="UTF-8" />
           <meta http-equiv="X-UA-Compatible" content="IE=edge" />
           <meta name="viewport" content="width=device-width, initial-scale=1.0" />
           <title>left right 和 width 实现水平居中</title>
           <style>
               img {
                   position: absolute;
                   right: 0;
                   left: 0;
                   width: 800px;
                   /* 开启决定定位后 margin: auto 是无法实现水平居中的，需要通过 left right 和 width 配合使用 */
                   margin: auto;
               }
           </style>
       </head>
       <body>
           <img src="../image/img.jpg" />
       </body>
   </html>
   ```

   实现效果如下所示:

   ![image-20210514223204545](http://img.seecode.cc//picgo/image-20210514223204545.png)

### 与 `z-index` 的关系

绝对定位的元素可以通过 `z-index` 控制层级显示，但是在开发过程中，需要我们的代码结构清晰，这个并不是必要的。使用准则如下：

1. 如果只有一个决定定位的元素，自然就不需要 `z-index` 来控制层级显示，该元素会自动覆盖普通元素。
2. 如果有两个绝对定位的元素，控制 DOM 流的前后顺序也能达到需要的覆盖效果，自然也不需要 `z-index` 属性。
3. 如果有多个绝对定位交错，这种事非常少见的，通过 DOM 流的顺序，和 `z-index: 1` 就可以实现
4. 如果是非弹框类的决定定位元素 `z-index` 的值是大于2的，必定 `z-index` 是冗余的，代码完全可以优化。



> **值得注意的是**`absolute` 属性值是不能与 `float` 共同使用的，当共同使用时，`float` 将会失效。

## relative 属性值