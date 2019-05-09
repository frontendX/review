# typeof和instanceof的区别
### typeof
> 在 JavaScript 中，判断一个变量的类型常常会用 `typeof` 运算符，在使用 `typeof` 运算符时采用引用类型存储值会出现一个问题，无论引用的是什么类型的对象，它都返回 `object`。

### instanceof
> `instanceof` 运算符用来测试一个对象在其原型链中是否存在一个构造函数的 prototype 属性。
语法：object instanceof constructor
参数：object（要检测的对象.）constructor（某个构造函数）
描述：instanceof 运算符用来检测 constructor.prototype 是否存在于参数 object 的原型链上。

### instanceof例子
```javascript
function Person (name) {
    this.name = name;
}

function Student () {

}

Student.prototype = Person.prototype;
Student.prototype.constructor = Student;

let s = new Student('Tom');
console.log(s instanceof Person); // 返回 true
```
