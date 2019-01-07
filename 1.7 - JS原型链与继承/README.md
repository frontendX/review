# JS原型链与继承

### 概念

先了解一下构造函数，原型和实例的关系：
> 每个构造函数（`constructor`）都有一个原型对象（`prototype`），原型对象都包含一个指向构造函数的指针，而实例（`instance`）都包含一个指向原型对象的内部指针。

先了解一下以下声明（简称：声明2）：
> 如果要引用对象（实例`instance`）的某个属性或方法，会首先在对象的内部寻找该属性，如果找不到，然后才在该对象的原型（`instance.prototype`）里去找这个属性。

如果让原型对象指向另一个类型的实例，那么会发生什么：
即：`constructor1.prototype = instance2`
根据**声明2**，如果引用 `constructor1` 构造的实例 `instance` 的某个属性 `p1`：
- 首先会在 `instance1` 内部属性中找；
- 接着会在 `instance1.__proto__(constructor1.prototype)` 中找一边，而 `constructor1.prototype` 实际上是 `instance2`，也就是说在 `instance2` 中寻找该属性 `p1`；
- 如果 `instance2` 中还是没有，它会继续在 `instance2.__proto__(constructor2.prototype)` 中寻找，直到 `Object` 的原型对象。

> 搜索轨迹: instance1--> instance2 --> constructor2.prototype…-->Object.prototype

这种搜索的轨迹，形成一条长链，又因 `prototype` 在这个游戏规则中充当链接的作用，于是我们把这种实例与原型的链条称作**原型链**。看下面例子：

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>JS原型链与继承</title>
    </head>
    <body>
        <script type="text/javascript">
            function Father() {
                this.property = "father";
            }
            Father.prototype.getFatherValue = function() {
                return this.property;
            }

            function Son() {
                this.sonProperty = "son";
            }
            //继承 Father
            Son.prototype = new Father();   //Son.prototype被重写，导致 Son.prototype.constructor 也一同被重写
            Son.prototype.getSonVaule = function() {
            	return this.sonProperty;
            }
            var instance = new Son();
            alert( instance.getFatherValue() ); // father
        </script>
    </body>
</html>
```

`instance` 实例通过原型链找到了 `Father` 原型中的 `getFatherValue` 方法。
**注意：**此时 `instance.constructor ` 指向的是 `Father`，这是因为 `Son.prototype` 中的 `constructor` 被重写。

### 借用构造函数

> 基本思想：即在子类型构造函数的内部调用超类型构造函数。

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>借用构造函数</title>
    </head>
    <body>
        <script type="text/javascript">
        function Father() {
            this.colors = ["red", "blue", "green"];
        }

        function Son() {
            Father.call( this );  // 继承了Father，且向父类型传递参数
        }

        var instance1 = new Son();
        instance1.colors.push( "black" );
        console.log( instance1.colors );    // ["red", "blue", "green", "black"]

        var instance2 = new Son();
        console.log( instance2.colors );    // ["red", "blue", "green"] - 可见引用类型值是独立的
        </script>
    </body>
</html>
```
借用构造函数有以下特点：
- 保证了原型链中引用类型的值的独立，不再被所有实例共享；
- 子类型创建时也能够向父类型传递参数。

存在的问题：
- 超类型(如Father)中定义的方法,对子类型而言也是不可见的；

### 组合继承
组合继承，有时候也叫做伪经典继承，指的是将原型链和借用构造函数的技术组合到一块，从而发挥两者之长的一种继承模式。

> 基本思路：使用原型链实现对原型属性和方法的继承，通过借用构造函数来实现对实例属性的继承。

这样，既通过在原型上定义方法实现了函数复用，又能保证每个实例都有它自己的属性。看下面例子：

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>组合继承</title>
    </head>
    <body>
        <script type="text/javascript">
            function Father( name ) {
                this.name = name;
                this.colors = ["red", "blue", "green"];
            }
            Father.prototype.sayName = function() {
                console.log( "name:", this.name );
            }

            function Son( name, age ) {
                Father.call( this, name );  // 继承实例属性，第一次调用Father()
                this.age = age;
            }
            Son.prototype = new Father();   // 继承父类方法，第二次调用Father()
            Son.prototype.sayAge = function() {
                console.log( "age:", this.age );
            }

            var instance1 = new Son( "cat", 5 );
            instance1.colors.push( "black" );
            console.log( instance1.colors );    // ["red", "blue", "green", "black"]
            instance1.sayName();    // name: cat
            instance1.sayAge();     // age: 5

            var instance1 = new Son( "dog", 1 );
            console.log( instance1.colors );    // ["red", "blue", "green"]
            instance1.sayName();    // name: dog
            instance1.sayAge();     // age: 1
        </script>
    </body>
</html>
```
