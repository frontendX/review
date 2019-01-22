# 面向对象程序设计
### 原型与 in 操作符
有两种方式使用 `in` 操作符:单独使用和在 `for-in` 循环中使用。在单独使用时，`in` 操作符会在通过对象能够访问给定属性时返回 `true`，无论该属性存在于实例中还是原型中。
```javascript
function Person() {}

Person.prototype = {
    name: "Nima",
    age: "22",
    job: 'Eat',
    sayName() {
        alert( this.name );
    }
};

var person1 = new Person();
var person2 = new Person();

alert(person1.hasOwnProperty("name"));  //false
alert("name" in person1);  //true

person1.name = "Greg";
alert(person1.name); //"Greg" ——来自实例
alert(person1.hasOwnProperty("name")); //true
alert("name" in person1); //true

alert(person2.name); //"Nicholas" ——来自原型
alert(person2.hasOwnProperty("name")); //false
alert("name" in person2); //true

delete person1.name;
alert(person1.name); //"Nicholas" ——来自原型
alert(person1.hasOwnProperty("name")); //false
alert("name" in person1); //true
```
在以上代码执行的整个过程中，`name` 属性要么是直接在对象上访问到的，要么是通过原型访问到 的。因此，调用 `"name" in person1` 始终都返回 `true`，无论该属性存在于实例中还是存在于原型中。 同时使用 `hasOwnProperty()` 方法和 `in` 操作符，就可以确定该属性到底是存在于对象中，还是存在于原型中，如下所示。

```javascript
function hasPrototypeProperty(object, name){
        return !object.hasOwnProperty(name) && (name in object);
}
```

由于 `in` 操作符只要通过对象能够访问到属性就返回 `true`，`hasOwnProperty()` 只在属性存在于实例中时才返回 `true`，因此只要 `in` 操作符返回 `true` 而 `hasOwnProperty()` 返回 `false`，就可以确 定属性是原型中的属性。下面来看一看上面定义的函数 `hasPrototypeProperty()` 的用法。

```javascript
function Person() {}

Person.prototype = {
    name: "Nima",
    age: "22",
    job: 'Eat',
    sayName() {
        alert( this.name );
    }
};

var person = new Person();
alert( hasPrototypeProperty(person, "name") );  //true

person.name = "Greg";
alert( hasPrototypeProperty(person, "name") );  //false
```

上面代码，`name` 属性先是存在于原型中，因此 `hasPrototypeProperty()` 返回 `true`。当在实例中 重写 `name` 属性后，该属性就存在于实例中了，因此 `hasPrototypeProperty()` 返回 `false`。即使原 型中仍然有 `name` 属性，但由于现在实例中也有了这个属性，因此原型中的 `name` 属性就用不到了。
