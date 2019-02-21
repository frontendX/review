# Javascript中的柯里化

### 什么是柯里化
如果一个函数可以接收多个参数，将这个函数转化为每次只接收一部分参数的函数的多次调用形式，就是柯里化。先看下面的代码：
```javascript
function add(a, b, c) {
    return a + b + c;
}
```
这个add函数接收3个参数，返回3个参数相加的结果。可以通过以下2种形式对其进行柯里化：
```javascript
function addOne(a) {
    return function(b) {
        return function(c) {
            return a + b + c;
        }
    }
}


function addTwo(a,b) {
    return function(c) {
        return a + b + c;
    }
}
```

```javascript
add(1, 2, 3);        // return 6
addOne(1)(2)(3);     // return 6
addTwo(1, 2)(3);     // return 6
```

如果使用ES6语法，能更简洁的写出柯里化后的函数，以addOne为例:
```javascript
const addOne = (a) => (b) => (c) => (a + b + c)
```

### 使用场景: 代码复用
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>Javascript中的柯里化</title>
    </head>
    <body>
        <script type="text/javascript">
            const match = reg => str => str.match( reg );
            const hasSpace = match( /\s+/g );
            const filter = fn => arr => arr.filter( fn );
            const findSpace = filter( hasSpace );
            let result = findSpace( ['hi man', 'hi_man'] );

            console.log( result );    // 输出： ['hi man']
        </script>
    </body>
</html>
```
