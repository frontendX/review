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

### 了解一下 `constructor` 构造函数
函数还有一种用法，就是把它作为构造函数使用。像 `Object` 和 `Array` 这样的原生构造函数，在运行时会自动出现在执行环境中。此外，也可以创建自定义的构造函数，从而自定义对象类型的属性和方法。如下代码所示：

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>constructor</title>
    </head>
    <body>
        <script type="text/javascript">
            function Animal( name, color ) {
                this.name = name;
                this.color = color;
                this.sayName = function() {
                    console.log( this.name );
                }
            }

            let animal1 = new Animal( "dog", "black" );
            animal1.sayName();
            let animal2 = new Animal( "cat", "white" );
            animal2.sayName();
        </script>
    </body>
</html>
```

在这个例子中，我们创建了一个自定义构造函数 `Animal()`，并通过该构造函数创建了两个普通对象 `animal1` 和 `animal2`，这两个普通对象均包含2个属性和1个方法。

你应该注意到函数名 `Animal` 使用的是大写字母 `P`。按照惯例，构造函数始终都应该以一个大写字母开头，而非构造函数则应该以一个小写字母开头。这个做法借鉴自其他面向对象语言，主要是为了区别于 JavaScript 中的其他函数；因为构造函数本身也是函数，只不过可以用来创建对象而已。

### new 操作符
要创建 `Animal` 的新实例，必须使用 `new` 操作符。以这种方式调用构造函数实际上会经历以下4个步骤：
- 1、创建一个新对象；
- 2、将构造函数的作用域赋给新对象（因此 `this` 指向了这个新的对象）；
- 3、执行构造函数中的代码（为这个新对象添加属性）；
- 4、返回新对象。

### 构造函数的问题
构造函数模式虽然好用，但也并非没有缺点。使用构造函数的主要问题，就是每个方法都要在每个实例上重新创建一遍。在前面的例子中，`animal1` 和 `animal2` 都有一个名为 `sayName()` 的方法，但那两个方法不是同一个 `Function` 的实例。因为 `JavaScript` 中的函数是对象，因此每定义一个函数，也就是实例化了一个对象。从逻辑角度讲，此时的构造函数也可以这样定义。

```javascript
<script type="text/javascript">
    function Animal( name, color ) {
        this.name = name;
        this.color = color;
        this.sayName = new Function("console.log(this.name)"); // 与声明函数在逻辑上是等价的
    }
</script>
```

从这个角度上来看构造函数，更容易明白每个 `Animal` 实例都包含一个不同的 `Function` 实例（`sayName()` 方法）。说得明白些，以这种方式创建函数，虽然创建 `Function` 新实例的机制仍然是相同的，但是不同实例上的同名函数是不相等的，以下代码可以证明这一点。

```javascript
console.log( animal1.sayName == animal2.sayName );  // false
```

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
