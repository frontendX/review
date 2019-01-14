# 理解用 async/await 来处理异步

### async的用法
`async` 作为一个关键字放到函数前面，用于表示函数是一个异步函数，因为 `async` 就是异步的意思， 异步函数也就意味着该函数的执行不会阻塞后面代码的执行。 写一个 `async` 函数。

```javascript
async function func() {
　　return 'hello world';
}
```

语法很简单，就是在函数前面加上 `async` 关键字，来表示它是异步的，那怎么调用呢？`async` 函数也是函数，平时我们怎么使用函数就怎么使用它，直接加括号调用就可以了，为了表示它没有阻塞它后面代码的执行，我们在 `async` 函数调用之后加一句 `console.log`;
```javascript
async function func() {
    return 'hello world'
}
func();     // 直接调用 func() ，控制台没有返回
console.log( '虽然在后面，但是我先执行...' );
```

控制输出：
```
// 输出
虽然在后面，但是我先执行...
```

`async` 函数 `func` 调用了，但是没有任何输出，它不是应该返回 `hello world`，先不要着急，看一看 `func()` 执行返回了什么？ 把上面的 `func()` 语句改为 `console.log(func())`；

```javascript
async function func() {
    return 'hello world'
}
console.log( func() );     //
console.log( '虽然在后面，但是我先执行...' );
```

控制输出：
```javascript
Promise {<resolved>: "hello world"}     // console.log( func() );
虽然在后面，但是我先执行...                 // console.log( '虽然在后面，但是我先执行...' );
```

原来 `async` 函数返回的是一个 `promise` 对象，如果要获取到 `promise` 返回值，我们应该用 `then` 方法， 继续修改代码：
```javascript
async function func() {
    return 'hello world';
}
func().then(result => {
    console.log(result);                // 后输出
});
console.log('虽然在后面，但是我先执行');    // 先输出
```

控制台输出：
```javascript
虽然在后面，但是我先执行
hello world
```

**获取到了 `hello world`,  同时 `func` 的执行也没有阻塞后面代码的执行。**

这时，你可能注意到控制台中的 `Promise` 有一个 `resolved`，这是 `async` 函数内部的实现原理。如果 `async` 函数中有返回一个值，当调用该函数时，内部会调用 `Promise. resolve()` 方法把它转化成一个 `promise` 对象作为返回，但如果 `func` 函数内部抛出错误呢？那么就会调用 `Promise.reject()` 返回一个 `promise` 对象， 这时修改一下 `func` 函数。
```javascript
async function func( flag ) {
    if( flag ) {
        return 'hello world'
    } else {
        throw 'my god, failure'
    }
}
console.log(func(true))  // 调用 Promise.resolve() 返回 promise 对象。
console.log(func(false)); // 调用 Promise.reject() 返回 promise 对象。
```

控制台输出：
```javascript
Promise {<resolved>: "hello world"}
Promise {<rejected>: "my god, failure"}
```

如果函数内部抛出错误， `promise` 对象有一个 `catch` 方法进行捕获。
```javascript
func(false).catch(err => {
    console.log( err );
});
```

### await的用法
`await` 是等待的意思，那么它等待什么呢，它后面跟着什么呢？其实它后面可以放任何表达式，不过我们更多的是放一个返回 `promise` 对象的表达式。注意 `await` 关键字只能放到 `async` 函数里面。

看例子：现在写一个函数，让它返回 `promise` 对象，该函数的作用是 2s 之后让数值乘以2
```javascript
// 2s 之后返回双倍的值
function doubleAfter2seconds(num) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve(2 * num)
        }, 2000);
    } );
}
```

现在再写一个 `async` 函数，从而可以使用 `await` 关键字， `await` 后面放置的就是返回 `promise` 对象的一个表达式，所以它后面可以写上 `doubleAfter2seconds` 函数的调用
```javascript
async function testResult() {
    let result = await doubleAfter2seconds(30);
    console.log(result);
}
```

控制台输出：
```javascript
testResult();

// 输出
2s 之后，输出了 60
```

现在我们看看代码的执行过程，调用 `testResult` 函数，它里面遇到了`await`，**`await` 表示等一下，代码就暂停到这里，不再向下执行了**，它等什么呢？等后面的 `promise` 对象执行完毕，然后拿到 `promise resolve` 的值并进行返回，返回值拿到之后，它继续向下执行。具体到 我们的代码, 遇到 `await` 之后，代码就暂停执行了， 等待 `doubleAfter2seconds(30)` 执行完毕，`doubleAfter2seconds(30)` 返回的 `promise` 开始执行，2秒之后，`promise resolve` 了， 并返回了值为60， 这时 `await` 才拿到返回值60， 然后赋值给 result， 暂停结束，代码才开始继续执行，执行 `console.log` 语句。

就这一个函数，我们可能看不出 `async/await` 的作用，如果我们要计算3个数的值，然后把得到的值进行输出呢？

```javascript
async function testResult() {
    let first = await doubleAfter2seconds(30);
    let second = await doubleAfter2seconds(50);
    let third = await doubleAfter2seconds(30);
    console.log( first + second + third );
}
```

**6秒后，控制台输出220**，我们可以看到，写异步代码就像写同步代码一样了，再也没有回调地域了。
