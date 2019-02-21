# 浏览器事件机制

### 1、事件传播的三个阶段：捕获，目标对象，冒泡

![eventflow](https://upload-images.jianshu.io/upload_images/1726248-448a22ad588da43d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 1.其中捕获（`Capture`）是 事件对象([event object](https://dom.spec.whatwg.org/#event)) 从 window 派发到 目标对象父级的过程。
- 2.目标（`Target`）阶段是 事件对象派发到目标元素时的阶段，如果事件类型指示其不冒泡，那事件传播将在此阶段终止。
- 3.冒泡（`Bubbling`）阶段 和捕获相反，是以目标对象父级到 window 的过程。
在任一阶段调用 `stopPropagation` 都将终止本次事件的传播。

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>浏览器事件机制</title>
        <style media="screen">
            #div3 {
                width: 100px;
                height: 100px;
                background-color: #000;
            }
        </style>
    </head>
    <body>
        <div id="div1">
            <div id="div2">
                <div id="div3">

                </div>
            </div>
        </div>

        <script type="text/javascript">
        let div1 = document.getElementById( "div1" );
        div1.addEventListener( 'click', e => {
            console.log( e );
        }, true );
        </script>
    </body>
</html>
```

**`addEventListener` 形式注册的监听事件接受参数以指定是否在捕获阶段触发本次事件，默认值为否(既冒泡阶段)。以事件处理器注册的事件在非捕获阶段触发。**

点击视窗的内任一元素，输出 `MouseEvent` 对象。查看对象的 `path` 属性，既该对象的传播路径。

![MouseEvent 对象](https://upload-images.jianshu.io/upload_images/1726248-913cf9ae810f0eb5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 2、阻止冒泡和默认事件
- 调用  `stopPropagation` 严格来说不是阻止冒泡，是阻止事件传播，捕获阶段也可以直接阻止。
- 事件接口还有一个 `cancelBubble` 因历史原因的 `stopPropagation` 的别名，给其赋值 true 可以达到调用 `stopPropagation` 同样的效果。
- 调用 `preventDefault` 则是阻止默认事件。

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>浏览器事件机制</title>
        <style media="screen">
            #div1 {
                width: 300px;
                height: 300px;
                background-color: #f00;
            }
            #div2 {
                width: 200px;
                height: 200px;
                background-color: #ff0;
            }
            #div3 {
                width: 100px;
                height: 100px;
                background-color: #000;
            }
        </style>
    </head>
    <body>
        <div id="div1">
            <div id="div2">
                <div id="div3">

                </div>
            </div>
        </div>

        <script type="text/javascript">
        let div1 = document.getElementById( "div1" );
        div1.addEventListener( 'click', e => {
            e.stopPropagation();    // 阻止事件传播
            console.log( e );
        }, true );

        let div3 = document.getElementById( "div3" );
        div3.addEventListener( 'click', e => {
            console.log( e );
        }, true );
        </script>
    </body>
</html>
```
