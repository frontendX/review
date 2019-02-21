# 白屏和首屏时间

### 白屏时间

白屏时间是指浏览器从响应用户输入网址地址，到浏览器开始显示内容的时间。
白屏时间 = 地址栏输入网址后回车 - 浏览器出现第一个元素

![白屏时间](https://upload-images.jianshu.io/upload_images/1726248-35f2c83b617ac4f6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 计算白屏时间

因此，我们通常认为浏览器开始渲染 `<body>` 标签或者解析完 `<head>` 标签的时刻就是页面白屏结束的时间点。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>白屏</title>
    <script type="text/javascript">
        // 不兼容performance.timing 的浏览器，如IE8
        window.pageStartTime = Date.now();
    </script>
    <!-- 页面 CSS 资源 -->
    <link type="text/css" rel="stylesheet" href="common.min.css" />
    <link type="text/css" rel="stylesheet" href="index.min.css" />
    <script type="text/javascript">
        // 白屏时间结束点
        window.firstPaint = Date.now();
    </script>
</head>
<body>
    <!-- 页面内容 -->
    <p>因此白屏时间则可以这样计算出：</p>

    <br/>
    <p>
    可使用 Performance API 时：<br/>
    白屏时间 = firstPaint - performance.timing.navigationStart;
    </p>

    <br/>
    <p>
    不可使用 Performance API 时：<br/>
    白屏时间 = firstPaint - pageStartTime;
    </p>
</body>
</html>
```

### 首屏时间

首屏时间是指浏览器从响应用户输入网络地址，到首屏内容渲染完成的时间。

首屏时间是指用户打开网站开始，到浏览器首屏内容渲染完成的时间。对于用户体验来说，首屏时间是用户对一个网站的重要体验因素。通常一个网站，如果首屏时间在5秒以内是比较优秀的，10秒以内是可以接受的，10秒以上就不可容忍了。超过10秒的首屏时间用户会选择刷新页面或立刻离开。

首屏时间 = 地址栏输入网址后回车 - 浏览器第一屏渲染完成

![首屏时间](https://upload-images.jianshu.io/upload_images/1726248-54467bf802122c62.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 计算首屏时间常用的方法有：
###### 1、首屏模块标签标记法
首屏模块标签标记法，通常适用于首屏内容不需要通过拉取数据才能生存以及页面不考虑图片等资源加载的情况。我们会在 HTML 文档中对应首屏内容的标签结束位置，使用内联的 JavaScript 代码记录当前时间戳。如下所示：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>首屏模块标签标记法</title>
    <script type="text/javascript">
        // 不兼容performance.timing 的浏览器，如IE8
        window.pageStartTime = Date.now();
    </script>
    <!-- 页面 CSS 资源 -->
    <link type="text/css" rel="stylesheet" href="common.min.css" />
    <link type="text/css" rel="stylesheet" href="index.min.css" />
    <script type="text/javascript">
        // 白屏时间结束点
        window.firstPaint = Date.now();
    </script>
</head>
<body>
    <!-- 首屏可见模块1 -->
    <div class="module-1"></div>
    <!-- 首屏可见模块2 -->
    <div class="module-2"></div>

    <script type="text/javascript">
        window.firstScreen = Date.now();
    </script>

    <!-- 首屏不可见模块3 -->
    <div class="module-3"></div>
    <!-- 首屏不可见模块4 -->
    <div class="module-4"></div>
</body>
</html>
```

###### 2、统计首屏内图片完成加载的时间/或者iframe
通常我们首屏内容加载最慢的就是图片资源，因此我们会把首屏内加载最慢的图片的时间当做首屏的时间。

由于浏览器对每个页面的 TCP 连接数有限制，使得并不是所有图片都能立刻开始下载和显示。因此我们在 DOM树 构建完成后将会去遍历首屏内的所有图片标签，并且监听所有图片标签 onload 事件，最终遍历图片标签的加载时间的最大值，并用这个最大值减去 navigationStart 即可获得近似的首屏时间。

此时：首屏时间 = 加载最慢的图片的时间点 - performance.timing.navigationStart;

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>统计首屏内图片完成加载的时间</title>
    <script>
        // 不兼容 performance.timing 的浏览器
        window.pageStartTime = Date.now()
    </script>
</head>
<body>
    <img src="xxx.jpg" alt="img" onload="load()">
    <img src="xxx.png" alt="img" onload="load()">
    <script>
        function load() {
            window.firstScreen = Date.now()
        }
        window.onload = function() {
            // 首屏时间
            console.log( window.firstScreen - performance.timing.navigationStart );
        }
    </script>
</body>
</html>
```
