# js对象可枚举和不可枚举属性

在 `JavaScript` 中，对象的属性分为可枚举和不可枚举之分，它们是由属性的 `enumerable` 值决定的。可枚举性决定了这个属性能否被 `for…in` 查找遍历到。
- **枚举**：是指对象中的属性是否可以遍历出来，再简单点说就是属性是否可以以列举出来。
- 在js中基本的数据类型是不能被枚举的。例如 `Object、Array、Number` 等。

### 1、`for…in` 循环可以枚举(遍历)出对象本身具有的属性，通过 `Object.defineProperty()` 方法加的可枚举属性，或者通过原型对象绑定的可以枚举属性。
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>对象可枚举和不可枚举属性</title>
    </head>
    <body>
        <script type="text/javascript">
            function Person() {
                this.name = "七猪";
                this.age = "1";
            }

            Person.prototype = {
                constructor: Person,
                job: "student",
            };

            // 其中用defineProperty为对象定义了一个名为”sex”的可枚举属性
            var stu = new Person();
            Object.defineProperty( stu, "sex", {
                value: "male",
                enumerable: true    // 设置为可枚举（如果是false，就是不可枚举）
            });

            for( var pro in stu ) {
                console.log("stu." + pro + " = " + stu[pro]);
            }

            // console.log( Object.keys(stu) );
        </script>
    </body>
</html>
```
![其中 sex 属性在代码中设置为可枚举](https://upload-images.jianshu.io/upload_images/1726248-8447e2adf6469aac.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 2、`Object.keys()` 方法可以枚举对象本身的属性和通过 `Object.defineProperty()` 添加的可枚举属性
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>对象可枚举和不可枚举属性</title>
    </head>
    <body>
        <script type="text/javascript">
            function Person() {
                this.name = "七猪";
                this.age = "1";
            }

            Person.prototype = {
                constructor: Person,
                job: "student",
            };

            // 其中用defineProperty为对象定义了一个名为”sex”的可枚举属性
            var stu = new Person();
            Object.defineProperty( stu, "sex", {
                value: "male",
                enumerable: true    // 设置为可枚举（如果是false，就是不可枚举）
            });

            // for( var pro in stu ) {
            //     console.log("stu." + pro + " = " + stu[pro]);
            // }

            console.log( Object.keys(stu) );
        </script>
    </body>
</html>
```

上面代码中，对象本身属性有 `name` 和 `age` 两个，再加上枚举了一个属性 `sex`，所以输出：
![Object.keys()](https://upload-images.jianshu.io/upload_images/1726248-640deb54ecd30007.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 3、`JSON.stringify()` 方法只能序列化本身的属性和通过 `Object.defineProperty()` 添加的可枚举属性为JSON对象
