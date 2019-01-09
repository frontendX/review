# 理解JSONP原理及使用

### 什么是JSOP
先了解一下 `JSON`：`JSON` 是一种轻量级的数据传输格式，被广泛应用于当前Web应用中。`JSON` 格式数据的编码和解析基本在所有主流语言中都被实现，所以现在大部分前后端分离的架构都以 `JSON` 格式进行数据的传输。

那什么是  `JSONP` 呢？首先抛出浏览器同源策略这个概念（**所谓同源是指：域名、协议、端口相同**），为了保证用户访问的安全，现代浏览器使用了同源策略，即不允许访问非同源的页面。在ajax中，不允许请求非同源的URL就可以了，比如 `www.a.com` 下的一个页面，其中的ajax请求是不允许访问 `www.b.com/c.php` 这样一个页面的。
`JSONP` 就是用来解决跨域请求问题的。

### JSOP原理
`ajax` 请求受同源策略影响，不允许进行跨域请求，而 `script` 标签 `src` 属性中的链接却可以访问跨域的js脚本，利用这个特性，服务端不再返回 `JSON` 格式的数据，而是返回一段调用某个函数的js代码，在src中进行了调用，这样实现了跨域。

### JSONP具体实现

1、在页面上发送一个 `ajax` 请求：
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>理解JSONP原理及使用</title>
    </head>
    <body>
        <script type="text/javascript" src="jquery-1.8.3.min.js"></script>
        <script type="text/javascript">
            function jsonhandle( data ) {
                console.log( data );
            }

            $(document).ready(function(){
                $.ajax({
                    type: "get",
                    async: false,
                    url: "your url",
                    type: "json",
                    success: function(res) {
                        callback( res );
                    }
                });
            });
        </script>
    </body>
</html>
```

当访问上面代码时， 浏览器会报以下错误，**提示了不同源的URL禁止访问**：

![报跨域的错](https://upload-images.jianshu.io/upload_images/1726248-782e152c54a7bdfc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2、下面使用 `JSONP`，将前端代码中的 `ajax` 请求去掉，添加了一个 `script` 标签，标签的src指向了另一个域 `www.balabala.com` 下的 `remote.js` 脚本：

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>理解JSONP原理及使用</title>
    </head>
    <body>
        <script type="text/javascript" src="jquery-1.8.3.min.js"></script>
        <script type="text/javascript">
        function jsonhandle( data ) {
            console.log( "name:" + data.name );
        }
        </script>
        <script type="text/javascript" src="http://www.balabala.com/remote.js"></script>
    </body>
</html>
```

假设 `remote.js` 脚本为：
```javascript
jsonhandle({
    "name": "轮毂",
})
```
当访问上面代码时，浏览器控制台会输出：
```javascript
name:轮毂
```

3、将前端代码改成动态加入 `script` ：
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>理解JSONP原理及使用</title>
    </head>
    <body>
        <script type="text/javascript" src="jquery-1.8.3.min.js"></script>
        <script type="text/javascript">
            function jsonhandle( data ) {
                console.log( data );
            }

            $(document).ready(function(){
                var url = "http://www.balabala.com/remote.php?id=1314&callback=jsonhandle";
                var obj = $('<script><\/script>');
                obj.attr( "src", url );
                $( "body" ).append( obj );
            });
        </script>
    </body>
</html>
```
上面代码动态的添加了一个 `script` 标签，`src` 指向跨域的一个php脚本，并且将上面的js函数名作为 `callback` 参数传入。

访问页面时，动态添加了一个 `script` 标签，src指向PHP脚本，执行返回的JS代码，成功返回数据。
综上：**`JSONP` 将访问跨域请求变成了执行远程JS代码，服务端不再返回JSON格式的数据，而是返回了一段将JSON数据作为传入参数的函数执行代码。**
