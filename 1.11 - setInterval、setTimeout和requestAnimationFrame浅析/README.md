# setInterval、setTimeout和requestAnimationFrame浅析

### 1、定时器
常见的定时器有以下两种：
```
window.setTimeout;    // 用于在指定的毫秒数后执行某段既定的代码
window.setInterval;    // 用于每隔一段毫秒数重复执行既定的代码
```
这两个方法都可以通过手工设置时间来设定是多少毫秒后执行这段代码，或者是每隔多少毫秒执行这段代码。

虽然我们期待浏览器按照我们设定的时间精确的执行代码，但是js却不能保证代码能恰好在那个时间点被运行，原因有两个。

- 大多数浏览器并没有精确到毫秒级别的触发事件，例如，我们设定某个函数在3毫秒后执行，在老版本的IE中，这个函数至少会在15毫秒以后执行。而在现代浏览器中，这个数值会短一点，但时间差一般也会超过1毫秒。
- 第二个原因与js的运行机制有关，具体见[JavaScript 运行机制详解：再谈Event Loop](http://www.ruanyifeng.com/blog/2014/10/event-loop.html).简单来说，就是js是一个单线程的解释器，一段时间只能执行一段代码，所以运行时分为主线程和任务队列两部分。而我们在定时器中设置的时间，仅代表1000毫秒后把这个任务插入到任务队列中，而此时必须要等到主线程的代码执行完毕，才能执行任务队列中的定时器的任务（在任务队列中也有调度，不一定第一个执行当前任务），因此时间是无法保证的。

### 2、requestAnimationFrame
与 setTimeout 相比，requestAnimationFrame 最大的优势是 **由系统来决定回调函数的执行时机**。具体一点讲就是，**系统每次绘制之前会主动调用 requestAnimationFrame 中的回调函数**，如果系统绘制率是 60Hz，那么回调函数就每16.7ms 被执行一次，如果绘制频率是75Hz，那么这个间隔时间就变成了 1000/75=13.3ms。换句话说就是，requestAnimationFrame 的执行步伐跟着系统的绘制频率走。**它能保证回调函数在屏幕每一次的绘制间隔中只被执行一次**，这样就不会引起丢帧现象，也不会导致动画出现卡顿的问题。
看一下示例：
```javasciprt
var progress = 0;
//回调函数
function render() {
    progress += 1; //修改图像的位置

    if (progress < 100) {
           //在动画没有结束前，递归渲染
           window.requestAnimationFrame(render);
    }
}

//第一帧渲染
window.requestAnimationFrame(render);
```

**requestAnimationFrame 还有以下特点**
**函数节流**：在高频率事件(resize,scroll 等)中，为了防止在一个刷新间隔内发生多次函数执行，使用 requestAnimationFrame 可保证每个绘制间隔内，函数只被执行一次，这样既能保证流畅性，也能更好的节省函数执行的开销。一个绘制间隔内函数执行多次时没有意义的，因为显示器每16.7ms 绘制一次，多次绘制并不会在屏幕上体现出来。

**兼容处理**
由于 requestAnimationFrame 目前还存在兼容性问题，而且不同的浏览器还需要带不同的前缀。因此需要通过优雅降级的方式对 requestAnimationFrame 进行封装，优先使用高级特性，然后再根据不同浏览器的情况进行回退，直止只能使用 setTimeout 的情况，因此可以这么写：
```javascript
window.requestAnimFrame = (function(){
  return  window.requestAnimationFrame       ||
          window.webkitRequestAnimationFrame ||
          window.mozRequestAnimationFrame    ||
          function( callback ){
            window.setTimeout(callback, 1000 / 60);
          };
})();
```
