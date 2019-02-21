# js拖拽：简单的元素拖拽
先介绍一下实现元素拖动需要的坐标属性：**`offsetLeft、offsetTop 和 clientX、clientY`**。

### offsetLeft 和 offsetTop
`offsetLeft 、 offsetTop` 用在dom节点中，可以返回当前元素距离某个父辈元素左边缘的距离
- 1、如果父辈元素中有定位的元素，那么就返回距离当前元素最近的定位元素边缘的距离。
- 2、如果父辈元素中没有定位元素，那么就返回相对于body左边缘距离。
```javascript
语法：obj.offsetLeft
```

### clientX 和 clientY
`clientX、clientY` 在事件中使用，返回当事件被触发时鼠标指针向对于浏览器页面（或客户区）的水平坐标。
```javascript
语法： event.clientX
```

![图有点丑](https://upload-images.jianshu.io/upload_images/1726248-a67461697018f422.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 实现原理
选择元素时（鼠标按下）保存鼠标的位置和元素位置的差值；移动元素时（鼠标移动），不停的给元素赋值（当前鼠标位置减去保存的差值）；释放元素（鼠标抬起）时，取消移动事件。

### 实现步骤
- 1、按下时保存差值：
```javascript
let disX = ev.clientX - dom.offsetLeft
let disY = ev.clientY - dom.offsetTop
```
- 2、移动时：
```javascript
dom.style.left = ev.clientX - disX + 'px';
dom.style.top = ev.clientY - disY + 'px';
```
- 3、释放元素时：
```javascript
dom.onmousemove = dom.onmouseup = null;
```

### 问题
- 1、移动太快脱离元素时事件就不会执行了：`onmousemove` 事件放在 `document` 上就可以了
- 2、抬起鼠标时在其他元素上抬起的，元素也不会停止：同样把 `onmouseup` 事件也放在 `document` 上

### 整体代码如下：
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>js拖拽</title>
        <style media="screen">
            * {
                margin: 0;
                padding: 0;
            }
            .drag {
                width: 100px;
                height: 100px;
                position: absolute;
                left: 100px;
                top: 100px;
                background-color: lightpink;
                border: 1px solid lightgreen;
            }
        </style>
    </head>
    <body>
        <div class="drag" id="drag">

        </div>

        <script type="text/javascript">
        let drag = document.getElementById( "drag" );

        drag.onmousedown = e => {
            let event = e || window.event;
            console.log( event );
            let disX = event.clientX - drag.offsetLeft;
            let disY = event.clientY - drag.offsetTop;

            console.log( "clientX:", event.clientX );
            console.log( "clientY:", event.clientY );
            console.log( "offsetLeft:", drag.offsetLeft );
            console.log( "offsetTop:", drag.offsetTop );
            console.log( disX, disY );

            // 鼠标移动时
            document.onmousemove = e => {
                let event = e || window.event;
                drag.style.left = `${event.clientX - disX}px`;
                drag.style.top = `${event.clientY - disY}px`;
            }

            // 鼠标抬起时
            document.onmouseup = () => {
                document.onmouseup = document.onmousemove = null;
            }
        }

        </script>
    </body>
</html>
```
