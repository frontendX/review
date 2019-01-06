# js闭包

### 1、闭包的概念

**闭包**就是能够读取其他函数内部变量的函数，函数没有被释放，整条作用域链上的局部变量都将得到保留。
由于在javascript语言中，只有函数内部的子函数才能读取局部变量，因此可以把闭包简单理解成定义在一个函数内部的函数。
所以，在本质上，闭包就是将函数内部和函数外部连接的一座桥梁。

### 2、闭包的用途
闭包可以用在许多地方。
##### 2.1、可以读取函数内部的变量，也可以让这些变量的值始终保持在内存中。

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>js闭包</title>
    </head>
    <body>
        <script type="text/javascript">
            function func1() {
                let n = 9;

                nAdd = function() {
                    n += 1;
                }

                function func2() {
                    console.log( n );
                }

                return func2;
            }

            var result = func1();
            result();　　//从函数外部通过闭包 func2 获取到函数 func1 内部局部变量的值
            nAdd();　　  //从函数外部通过闭包修改局部变量n的值
            result();　　//再次通过闭包 func2 获取到函数 func1 内部局部变量的值
        </script>
    </body>
</html>
```

在这段代码中，`result`实际上就是闭包 `func2` 函数。它一共运行了两次，第一次的值是9，第二次的值是10。这证明了，函数 `func1` 中的局部变量 `n` 一直保存在内存中，并没有在 `func1` 调用后被自动清除。

为什么会这样呢？原因就在于 `func1` 是 `func2` 的父函数，而 `func2` 被赋给了一个全局变量，这导致 `func2` 始终在内存中，不会再调用结束后，被垃圾回收机制(garbage collection)回收。

这段代码中另一个值得注意的地方，就是 `nAdd=function(){n+=1}` 这一行，首先在 `nAdd` 前面没有使用 `var` 关键字，因此 `nAdd` 是一个全局变量，而不是局部变量。其次，`nAdd` 的值是一个匿名函数(anonymous function)，而这个匿名函数本身也是一个闭包，所以 `nAdd` 相当于是一个setter，可以在函数外部对函数内部的局部变量进行操作。

##### 2.2、模拟面向对象的代码风格
封装：

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>js闭包 - 模拟面向对象的代码风格</title>
    </head>
    <body>
        <script type="text/javascript">
            const person = function() {
                let name = "balabala";

                return {
                    getName() {
                        return name;
                    },

                    setName( newName ) {
                        name = newName;
                    }
                }
            }();

            console.log( person.name );//直接访问，结果为undefined
            console.log( person.getName() );    // 输出：balabala
            person.setName( "diudiudiu" );
            console.log( person.getName() );    // 输出：diudiudiu
        </script>
    </body>
</html>
```

##### 2.3、用闭包模仿块级作用域

用 `var` 定义变量存在变量提升问题，例如：

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>用闭包模仿块级作用域</title>
    </head>
    <body>
        <script type="text/javascript">
        for( var i = 0; i < 10; i++ ) {
            console.info( i );
        }
        alert( i );  // 变量提升，弹出10

        //为了避免i的提升可以这样做
        (function () {
            for( var j = 0; j < 10; j++ ) {
                console.info( j );
            }
        })();
        alert( j );   // Uncaught ReferenceError: j is not defined
                      // 因为i随着闭包函数的退出，执行环境销毁，变量回收
        </script>
    </body>
</html>
```

### 2.4、this指向问题
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>this指向问题</title>
    </head>
    <body>
        <script type="text/javascript">
        var person = {
            name: "balabala",
            getName() {
                return function() {
                    console.info( this.name );
                }
            }
        };
        person.getName()();    // underfined
                             // 因为里面的闭包函数是在window作用域下执行的，也就是说，this指向window

        </script>
    </body>
</html>
```
