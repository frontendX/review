# js里的__proto__和prototype到底有什么区别

### 声明：
> 1、在JS里，万物皆对象。方法（`Function`）是对象，方法的原型（`Function.prototype`）是对象。因此，它们都会具有对象共有的特点。即：对象具有属性`__proto__`，可称为隐式原型，一个对象的隐式原型指向构造该对象的构造函数的原型，这也保证了实例能够访问在构造函数原型中定义的属性和方法。

> 2、方法(Function)这个特殊的对象，除了和其他对象一样有上述`__proto__`属性之外，还有自己特有的属性——原型属性（`prototype`），这个属性是一个指针，指向一个对象，这个对象的用途就是包含所有实例共享的属性和方法（我们把这个对象叫做原型对象）。原型对象也有一个属性，叫做 `constructor`，这个属性包含了一个指针，指回原构造函数。


1.1、声明一个对象和一个函数，打印一下对象和函数的`__proto__`

```javascript
let A = function() {};
let B = {};

console.log( "A函数", A.__proto__ );
console.log( "B对象", B.__proto__ );
```
![__proto__](https://upload-images.jianshu.io/upload_images/1726248-952d7d22aae6110d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

以上**A函数**和**B对象**的`__proto__`为啥不一样呢？再看一下下面的分析：
```javascript
let A = function() {};
let B = {};

console.log( "A函数", A.__proto__.__proto__ );
console.log( "B对象", B.__proto__ );
```
![__proto__](https://upload-images.jianshu.io/upload_images/1726248-be506040a0ce3c15.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

注：**隐式原型指向构造该对象的构造函数的原型。**因为 `function` 是特殊的对象，`A.__proto__`就指向了构造该函数的一个函数（随意起个名字 C）,C的`__proto__`就指向了和B对象一样的`__proto__`。

1.2、声明一个对象和一个函数，打印一下对象和函数的`prototype `

```javascript
let A = function() {};
let B = {};

console.log( "A函数", A.prototype );
console.log( "B对象", B.prototype );
```
![prototype](https://upload-images.jianshu.io/upload_images/1726248-a55322223666efbb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

注：对象并不具有 `prototype` 属性，只有函数才有 `prototype` 属性。这就证明**声明2**的说法是正确的。

### 总结：
 - 1、js里所有的对象都有`__proto__`属性(对象，函数)，指向构造该对象的构造函数的原型。
- 2、只有函数function才具有`prototype`属性。这个属性是一个指针，指向一个对象，这个对象的用途就是包含所有实例共享的属性和方法（我们把这个对象叫做原型对象）。原型对象也有一个属性，叫做 `constructor`，这个属性包含了一个指针，指回原构造函数。
