# 【不一样的CSS】深入理解 position (掌握定位就是这么 sao 气)

[toc]


## 写在前面

对 CSS 布局掌握程度决定你在 Web 开发中的开发页面速度。随着 Web 技术的不断革新，实现各种布局的方式已经多得数不胜数了。

最近利用碎片时间，大概用了半个月的时间整理了一个系列，本系列文章总结了 CSS 中的各种布局，以及实现方式及其常用技巧。让你通过该系列文章对 CSS 布局有一个新的认识。

该系列的导航帖[点我进入](https://juejin.cn/post/6963251091035291656/)，里面可以快速跳转到你想了解的文章(建议收藏)

### `position` 属性概述

`position` 属性是掌握 CSS 布局中重要得一个属性，该属性用于指定一个元素在文档中的定位方式。通过 `top`，right，`bottom` 和 `left` 属性则决定了该元素的最终位置。

该属性具有以下五个值：

- `static`: 默认值，表示正常布局行为，此时设置  `top`, `right`, `bottom`, `left` 和 `z-index ` 属性均无效。
- `relative`： **将元素设置为相对定位元素**，该方式不脱离文档流
- `absolute`：  **将元素设置为绝对定位元素**，使元素相对于最近的非 `static` 定位祖先元素的进行定位。
- `fixed`: **将元素设置为固定定位元素**，使元素相对于视觉窗口进行定位。
- `sticky`: **将元素设置为粘性定位元素**，一开始不脱离文档流在默认位置，当到达某个位置时，相对于视口进行定位。

## absolute 属性值

### 基本特性

`absolute` 属性值与 `float` 属性名，具有相同的特性：

1. 包裹性：所谓的包裹性就是指元素的宽度会收缩到与内容一致。
2. 破坏性：所谓的破坏性指的就是父元素的高度塌陷

### 脱离文档流

将 `position` 属性的值设置为 `absolute` 时，其元素会脱离文档流。

文档流就是将窗体自上而下分成一行一行，并在每行中按从左至右依次排放元素，称为文档流，也称为普通流。

所谓的脱离文档流就是元素不再在文档流中占据空间，而是处于浮动状态 (可以理解为漂浮在文档流的上方) 。脱离文档流的元素的定位基于正常的文档流，当一个元素脱离文档流后，依然在文档流中的其他元素将忽略该元素并填补其原先的空间。

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

> **值得注意的是**`absolute` 属性值是不能与 `float` 共同使用的，当共同使用时，`float` 将会失效。

### 与 `z-index` 的关系

绝对定位的元素可以通过 `z-index` 控制层级显示，但是在开发过程中，需要我们的代码结构清晰，这个并不是必要的。使用准则如下：

1. 如果只有一个决定定位的元素，自然就不需要 `z-index` 来控制层级显示，该元素会自动覆盖普通元素。
2. 如果有两个绝对定位的元素，控制 DOM 流的前后顺序也能达到需要的覆盖效果，自然也不需要 `z-index` 属性。
3. 如果有多个绝对定位交错，这种事非常少见的，通过 DOM 流的顺序，和 `z-index: 1` 就可以实现
4. 如果是非弹框类的决定定位元素 `z-index` 的值是大于2的，必定 `z-index` 是冗余的，代码完全可以优化。

## relative 属性值

### 对 `absolute` 的限制作用

relative 属性值对 对 `absolute` 的具有如下限制作用

1. 当父元素开启 `relative` 定位后，其子元素如果开启 `absolute` 定位后 子元素的 `left`、right、`top`、`bottom` 的属性值永远不会超出父元素。

   示例代码如下所示：

   ```html
   <!DOCTYPE html>
   <html lang="en">
       <head>
           <meta charset="UTF-8" />
           <meta http-equiv="X-UA-Compatible" content="IE=edge" />
           <meta name="viewport" content="width=device-width, initial-scale=1.0" />
           <title>relative 属性值对 对 `absolute` 的具有如下限制作用</title>
           <style>
               .container {
                   /* 父级元素开启 relative 定位 */
                   position: relative;
                   height: 400px;
                   width: 600px;
                   margin: 0 auto;
                   background-color: #9c27b0;
               }
               .item {
                   /* 子元素开启  absolute 定位 */
                   position: absolute;
                   background-color: #4caf50;
                   height: 200px;
                   width: 300px;
                   /* 子元素的 top left right bottom 会被限制在 父级容器中 */
                   right: 0;
                   bottom: 0;
               }
           </style>
       </head>
       <body>
           <div class="container">
               <div class="item"></div>
           </div>
       </body>
   </html>
   ```

   执行结果如下所示：

   ![image-20210515085225610](http://img.seecode.cc//picgo/image-20210515085225610.png)

2. 当有两个元素都开启 `relative` 定位，并都通过 `z-index` 限制层级，假如父元素1号的 `z-index` 的值为2，父元素2号的 `z-index` 的值为1。父元素2号下面的子元素无论 `z-index` 的值多大，永远不会覆盖父元素1下面的子元素。

3. 如果父元素未开启 `relative` 定位，其 `overflow: hidden;` 的限制对 `absolute` 定位的子元素并没有任何限制，只有父元素开启  `relative` 定位后，才会对  `absolute` 定位的子元素做限制。

   示例代码如下：

   ```css
   .container {
       /* 父级元素开启 relative 定位 */
       position: relative;
       height: 400px;
       width: 600px;
       margin: 0 auto;
       background-color: #9c27b0;
       overflow: hidden;
   }
   .item {
       /* 子元素开启  absolute 定位 */
       position: absolute;
       background-color: #4caf50;
       height: 200px;
       width: 300px;
       right: 0;
       top: 0;
   }
   ```

   > HTML 结构同上

   当父元素未开启 relative 定位的效果如下：

   ![image-20210515090413687](http://img.seecode.cc//picgo/image-20210515090413687.png)

   开启后如下所示：

   ![image-20210515090427261](http://img.seecode.cc//picgo/image-20210515090427261.png)

###  left、right、top、bottom 的使用

当元素开启 `relative` 定位后，其偏移是相对于自身，且对其他元素没有侵入性的。简单的说就是相对自己原来的位置进行偏移，且对其他元素与没有任何的影响。

示例代码如下：

```html
  <!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta http-equiv="X-UA-Compatible" content="IE=edge" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>left等偏移属性的使用</title>
        <style>
            .container {
                /* 父级元素开启 relative 定位 */
                position: relative;
                height: 400px;
                width: 600px;
                background-color: #9c27b0;
                left: 100px;
                top: 100px;
            }
        </style>
    </head>
    <body>
        <div class="container"></div>
    </body>
</html>
```

执行结果如下：

![image-20210515091248757](http://img.seecode.cc//picgo/image-20210515091248757.png)

### realtive 最小化影响原则

所谓 relative 的最小化影响原则，指的是尽量 `relative` 属性值对其他元素或布局的潜在影响！具体体现在两个方面：

1. 使用 `absolute` 属性值时，尽量不使用 `relative` 做限制，而是使用 `margin` 完成其定位功能。

2. 当必须通过 父元素开启 `relative` 定位，子元素的 `absolute` 才能实现定位效果的时候，可以再其子元素单独创建一个的父元素，并开启 `relative` 定位，子元素就可以相对于该父元素进行定位，这么做的好处就是 `relative` 布局就不会影响其他元素。

   示例代码如下: 

   ```html
   <!DOCTYPE html>
   <html lang="en">
       <head>
           <meta charset="UTF-8" />
           <meta http-equiv="X-UA-Compatible" content="IE=edge" />
           <meta name="viewport" content="width=device-width, initial-scale=1.0" />
           <title>relative 最小化影响原则</title>
           <style>
               .container {
                   height: 400px;
                   width: 600px;
                   background-color: #9c27b0;
                   margin: 0 auto;
               }
               .item {
                   height: 100px;
                   width: 50px;
                   background-color: #ff7043;
                   line-height: 100px;
                   font-size: 30px;
                   text-align: center;
               }
           </style>
       </head>
       <body>
           <div class="container">
               <!-- 单独创建一个 开启 relative 定位的 父元素-->
               <div style="position: relative">
                   <!-- 这样不会影响后面元素的定位 -->
                   <div class="item" style="position: absolute; right: 0">1</div>
               </div>
               <div class="item">2</div>
           </div>
       </body>
   </html>
   ```

   执行结果如图所示：

   ![image-20210515093138396](http://img.seecode.cc//picgo/image-20210515093138396.png)

## fixed 属性值

当 `position` 的属性值设置为 `fixed` 时，该元素会相对于浏览器窗口进行偏移，即定位的基准点就是浏览器窗口。这就会导致该元素的位置不会跟随页面滚动而变化，就像固定在页面上一样。

该属性还可以搭配 `top`、`bottom`、`left`、`right` 这四个属性一起使用，表示该元素的初始位置是基于视口计算的。如果不使用这四个属性，表示元素的起始位置就是元素的默认位置。

示例代码如下所示：

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta http-equiv="X-UA-Compatible" content="IE=edge" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>fixed 属性值</title>
        <style>
            body {
                margin: 0;
                height: 2000px;
            }
            .container {
                /* 开启 fixed 定位 */
                position: fixed;
                height: 400px;
                background: #2c80c5;
                /* 实现居中 */
                left: 0;
                right: 0;
                width: 500px;
                margin: auto;
            }
        </style>
    </head>
    <body>
        <div class="container"></div>
    </body>
</html>
```

> 当页面无论怎么滚动，该元素始终定位在顶部。

执行结果如下：

![image-20210515211240930](http://img.seecode.cc//picgo/image-20210515211240930.png)

## sticky 属性值

`sticky` 属性值与其他属性值不一样，该属性值会产生一个动态效果，灵活的运用该属性值可以完成一个吸顶的效果。

这个值像是 `relative` 和 `fixed`的结合。一些时候是 `relative` 定位 (定位的基准点是自身默认位置) ，另一些时候自动变成 `fixed` 定位 (定位的基准点是视口) 。

该属性值必须搭配 `top`、`bottom`、`left`、`right` 这四个属性一起使用，不能省略，否则等同于 `relative` 定位，不产生 **动态固定** 的效果。

该属性值得定位规则为：当页面滚动，父元素开始脱离视口时即部分不可见) ，只要与 `sticky` 元素的距离达到生效门槛，`relative` 定位自动切换为 `fixed` 定位；等到父元素完全脱离视口时 (即完全不可见) ，`fixed` 定位自动切换回 `relative` 定位。

示例代码如下：

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta http-equiv="X-UA-Compatible" content="IE=edge" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>sticky 属性值</title>
        <style>
            .container {
                background: #eee;
                width: 600px;
                height: 1000px;
                margin: 0 auto;
            }
            .title {
                /* 适配  safari 浏览器 */
                position: -webkit-sticky;
                position: sticky;
                height: 60px;
                background: #ff7300;
                top: 0px;
                font-size: 30px;
                text-align: center;
                color: #fff;
                line-height: 60px;
            }
            img {
                width: 100%;
                display: block;
            }
        </style>
    </head>
    <body>
        <div class="container">
            <div>
                <div class="title">内容1</div>
                <img src="../image/img.jpg" />
            </div>
            <div>
                <div class="title">内容2</div>
                <img src="../image/img.jpg" />
            </div>
            <div>
                <div class="title">内容3</div>
                <img src="../image/img.jpg" />
            </div>
            <div>
                <div class="title">内容4</div>
                <img src="../image/img.jpg" />
                <img src="../image/img.jpg" />
            </div>
        </div>
    </body>
</html>
```

执行结果如下图所示：

![](http://img.seecode.cc//picgo/sticky%20%E5%B1%9E%E6%80%A7%E5%80%BC%E7%A4%BA%E4%BE%8B%E6%95%88%E6%9E%9C.gif)

当我们的页面滚动时，只要视口的顶部与 `.title` 的距离 `>=0`,`.title` 就会自动变为 `fixed` 定位，保持与视口顶部 `0px` 的距离。页面继续向下滚动，父元素彻底离开视口（即整个父元素完全不可见），`.title` 恢复成 `relative` 定位。

### 生效规则

`position:sticky` 的生效是有一定的限制的，总结如下：

1. 须指定 `top`, `right`, `bottom` 或 `left` 四个阈值其中之一，才可使粘性定位生效。否则其行为与相对定位相同。
   - 并且 `top` 和 `bottom` 同时设置时，`top` 生效的优先级高，`left` 和 `right` 同时设置时，`left` 的优先级高。
2. 设定为 `position:sticky` 元素的任意父节点的 overflow 属性必须是 visible，否则 `position:sticky` 不会生效。这里需要解释一下：
   - 如果 `position:sticky` 元素的任意父节点定位设置为 `overflow:hidden`，则父容器无法进行滚动，所以 `position:sticky` 元素也不会有滚动然后固定的情况。
   - 如果 `position:sticky` 元素的任意父节点定位设置为 `position:relative | absolute | fixed`，则元素相对父元素进行定位，而不会相对视觉窗口定位。
3. 达到设定的阀值。这个还算好理解，也就是设定了 `position:sticky` 的元素表现为 `relative` 还是 `fixed` 是根据元素是否达到设定了的阈值决定的。