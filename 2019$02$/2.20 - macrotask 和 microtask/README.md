# macrotask 和 microtask
看下面代码的运行结果：

```javascript
console.log('a');
setTimeout(() => {
    console.log('b');
}, 0);
console.log('c');
Promise.resolve().then(() => {
    console.log('d');
})
.then(() => {
    console.log('e');
});

console.log('f');
```

**答案：** 输出结果为 acfdeb，关于 `macrotask` 和 `microtask` 可以看一下盗的一张图。

![image.png](https://upload-images.jianshu.io/upload_images/1726248-5a0a714c823aa58b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
