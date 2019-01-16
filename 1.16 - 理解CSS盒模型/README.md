# 理解CSS盒模型
### css的两种盒模型 - W3C标准盒模型
![W3C标准盒模型](https://upload-images.jianshu.io/upload_images/1726248-4f05ba6d1619fbf0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 在标准的盒子模型中，`width` 指 `content` 部分的宽度

### css的两种盒模型 - IE盒模型
![IE盒模型](https://upload-images.jianshu.io/upload_images/1726248-8f74a45854b01cd2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 在IE盒子模型中，`width` 表示 `content+padding+border` 这三个部分的宽度

### 再看代码 - W3C标准盒模型
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>理解CSS盒模型 - W3C标准盒模型</title>
        <style media="screen">
            * {
                margin: 0;padding: 0;
            }
            .drag {
                width: 100px;
                height: 100px;
                background-color: lightpink;
                border: 5px solid lightgreen;
                padding: 10px;
                margin: 20px;
                box-sizing: content-box;  /* W3C盒子模型 */
                /* box-sizing: border-box;  /* IE盒子模型 */ */
            }
        </style>
    </head>
    <body>
        <div class="drag" id="drag"></div>

        <script type="text/javascript">
            let drag = document.getElementById( "drag" );
            console.log( drag.offsetWidth );
        </script>
    </body>
</html>
```
![W3C标准盒模型](https://upload-images.jianshu.io/upload_images/1726248-ac891648db92276e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

从上面可以看出，设置的 `100*100` 出现在了最里面的那个蓝框中，与此同时我们可以发现在这个盒模型中除了我们设置的内容（`content`），还有 `margin`（外边距）、`border`（边框）、`padding`（内边框）

- `margin`(外边距) - 清除边框外的区域，外边距是透明的。
- `border`(边框) - 围绕在内边距和内容外的边框。
- `padding`(内边距) - 清除内容周围的区域，内边距是透明的。
- `content`(内容) - 盒子的内容，显示文本和图像。

**根据上面提到的W3C标准盒模型的概念，所以：`width(100px) = content(100)`**

### 再看代码 - IE盒模型
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>理解CSS盒模型</title>
        <style media="screen">
            * {
                margin: 0;padding: 0;
            }
            .drag {
                width: 100px;
                height: 100px;
                background-color: lightpink;
                border: 5px solid lightgreen;
                padding: 10px;
                margin: 20px;
                /* box-sizing: content-box;  /* W3C盒子模型 */
                box-sizing: border-box;  /* IE盒子模型 */
            }
        </style>
    </head>
    <body>
        <div class="drag" id="drag"></div>

        <script type="text/javascript">
            let drag = document.getElementById( "drag" );
            console.log( drag.style );
        </script>
    </body>
</html>
```
![IE盒模型](https://upload-images.jianshu.io/upload_images/1726248-8c9fd78b5775f0c7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**根据上面提到的IE盒模型的概念，所以：`width(100px) = content(70) + padding(10*2) + border(5*2)`**

### box-sizing的使用
如果想要切换盒模型也很简单，这里需要借助 `css3` 的 `box-sizing` 属性，`box-sizing` 的默认属性是 `content-box`
```css
box-sizing: content-box;    是W3C盒子模型
box-sizing: border-box;     是IE盒子模型
```
