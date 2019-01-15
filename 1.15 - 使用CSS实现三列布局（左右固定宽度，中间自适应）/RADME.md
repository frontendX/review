# 使用CSS实现三列布局（左右固定宽度，中间自适应）

### 1、使用自动浮动法
自身浮动法的原理就是对左右分别使用 `float:left` 和 `float:right`，`float` 使左右两个元素脱离文档流，中间元素正常在正常文档流中。对中间文档流使用 `margin` 指定左右外边距进行定位。

该布局法的不足是三个元素的顺序，**`middle` 一定要放在最后**，`middle` 占据文档流位置，所以一定要放在最后，左右两个元素位置没有关系。当浏览器窗口很小的时候，右边元素会被挤到下一行。
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>使用自动浮动法</title>
        <style media="screen">
            * {
                margin: 0;padding: 0;
            }
            .main {
                width: 100%;height: 500px;
            }
            .left {
                width: 200px;height: 100%;background: red;float: left;
            }
            .right {
                width: 120px;height: 100%;background: red;float: right;
            }
            .middle {
                height: 100%;margin: 0 120px 0 200px;background: green;
            }
        </style>
    </head>
    <body>
        <section class="main">
            <section class="left">
                left
            </section>
            <section class="right">
                right
            </section>
            <section class="middle">
                middle
            </section>
        </section>
    </body>
</html>
```

### 2、使用绝对定位法
绝对定位法原理是将左右两边使用 `absolute` 定位，因为绝对定位使其脱离文档流，后面的 `middle` 会自然流动到他们上面，然后使用 `margin` 属性，留出左右元素的宽度，既可以使中间元素自适应屏幕宽度。

该法布局的好处，三个div顺序可以任意改变。但是因为是绝对定位，如果页面上还有其他内容，top的值需要小心处理。
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>使用绝对定位法</title>
        <style media="screen">
            * {
                margin: 0;padding: 0;
            }
            .main {
                width: 100%;height: 500px;position: relative;
            }
            .left {
                width: 200px;height: 100%;background: red;
                position: absolute;top: 0;left: 0;
            }
            .right {
                width: 120px;height: 100%;background: red;
                position: absolute;top: 0;right: 0;
            }
            .middle {
                height: 100%;margin: 0 120px 0 200px;background: green;
            }
        </style>
    </head>
    <body>
        <section class="main">
            <section class="left">
                left
            </section>
            <section class="right">
                right
            </section>
            <section class="middle">
                middle
            </section>
        </section>
    </body>
</html>
```

### 3、使用flex布局
设置一个父容器，添加样式 `display:flex`。中间div设置 `flex-grow:1`，（等同代码中设置 `flex:1`，`flex` 是 `grow、shrink、basis` 的简写）但是盒模型默认紧紧挨着，可以使用 `margin` 控制外边距。`middle` 一定在中间，否则需要 `order` 属性来调整。
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>使用flex布局</title>
        <style media="screen">
            * {
                margin: 0;padding: 0;
            }
            .main {
                width: 100%;height: 500px;
                display: flex;
            }
            .left {
                width: 200px;height: 100%;background: lightpink;
            }
            .right {
                width: 120px;height: 100%;background: lightpink;
            }
            .middle {
                height: 100%;background: lightgreen;
                flex: 1;
            }
        </style>
    </head>
    <body>
        <section class="main">
            <section class="left">
                left
            </section>
            <section class="middle">
                middle
            </section>
            <section class="right">
                right
            </section>
        </section>
    </body>
</html>
```

### 4、圣杯布局
圣杯布局的原理是 `margin` 负值法。首先设置父容器的位置，使其左右分别空出200px和120px区域。然后利用三列全部左浮动和相对定位以及设置 `left` 和 `right` 负的外边距可以实现。
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>圣杯布局</title>
        <style media="screen">
            * {
                margin: 0;padding: 0;
            }
            .main {
                padding: 0 120px 0 200px;
            }
            .left, .right, .middle {
                position: relative;min-height: 300px;float: left;
            }
            .left {
                width: 200px;background: red;
                left: -200px;margin-left: -100%;
            }
            .right {
                width: 120px;background: red;
                right: -120px;margin-left: -120px;
            }
            .middle {
                width: 100%;background: green;
            }
        </style>
    </head>
    <body>
        <section class="main">
            <section class="middle">
                middle
            </section>
            <section class="left">
                left
            </section>
            <section class="right">
                right
            </section>
        </section>
    </body>
</html>
```
