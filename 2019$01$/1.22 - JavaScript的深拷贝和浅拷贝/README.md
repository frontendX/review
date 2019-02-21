# JavaScript的深拷贝和浅拷贝
### 基本数据类型
它们的值在内存中占据着固定大小的空间，并被**保存在栈内存**中。当一个变量向另一个变量复制基本类型的值，会创建这个值的副本，并且我们不能给基本数据类型的值添加属性。

```javascript
let x = 1;
let y = x;
x.name = 'haha';
console.log( y ); //1
console.log( x.name ); //undefined
```

上面的代码中，`x` 是基本数据类型(`Number`)， `y` 是 `x` 的一个副本，它们两者都占有不同位置但相等的内存空间，只是它们的值相等，若改变其中一方，另一方不会随之改变。

### 引用类型
复杂的数据类型即是引用类型，它的值是对象，**保存在堆内存**中，包含引用类型值的变量实际上包含的不是对象本身，而是一个指向该对象的指针。从一个变量向另一个变量复制引用类型的值，复制的其实是指针地址而已，因此两个变量最终都指向同一个对象。

```javascript
let obj = {
   name:'Hanna Ding',
   age: 22
}
let obj2 = obj;
obj2['me'] = 'hee';
console.log( obj ); //Object {name: "Hanna Ding", age: 22, me: "hee"}
console.log( obj2 ); //Object {name: "Hanna Ding", age: 22, me: "hee"}
```

上面的代码中看到 `obj` 赋值给 `obj2` 后，但我们改变其中一个对象的属性值，两个对象都发生了改变，根本原因就是 `obj` 和 `obj2` 两个变量都指向同一个指针，赋值时只是复制了指针地址，它们指向同一个引用，所以当我们改变其中一个的值就会影响到另一个变量的值。

### 浅拷贝
有时候我们只是想备份数组，但是只是简单让它赋给一个变量，改变其中一个，另外一个就紧跟着改变，但很多时候这不是我们想要的。

```javascript
let arr = [1, 2, 3];

let arr2 = arr;
arr2[1] = "test";
console.log(arr); // [1, "test", 3]
console.log(arr2); // [1, "test", 3]
```

### 深拷贝
- 数组

    `concat` 方法跟 `slice` 方法拷贝只能深拷贝第一层，更深的属性拷贝不了。

- 对象

    对象的深拷贝实现原理： 定义一个新的对象，遍历源对象的属性并赋给新对象的属性。

封装一个深拷贝的函数：
```javascript
function deepClone( obj ) {
    let objClone = Array.isArray( obj ) ? [] : {};
    if( obj && typeof obj==="object" ) {
        for( key in obj ) {
            if( obj.hasOwnProperty(key) ) {
                //判断ojb子元素是否为对象，如果是，递归复制
                if( obj[key] && typeof obj[key] === "object" ) {
                    objClone[key] = deepClone( obj[key] );
                } else {
                    //如果不是，简单复制
                    objClone[key] = obj[key];
                }
            }
        }
    }
    return objClone;
}

let a = [1, 2, 3, 4];
let b = deepClone(a);
a[0] = 2;
console.log( a, b );
```
