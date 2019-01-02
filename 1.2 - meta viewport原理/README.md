# meta viewport原理

### 什么是Viewport
手机浏览器是把页面放在一个虚拟的“窗口”（viewport）中，通常这个虚拟的“窗口”（viewport）比屏幕宽，这样就不用把每个网页挤到很小的窗口中（这样会破坏没有针对手机浏览器优化的网页的布局），用户可以通过平移和缩放来看网页的不同部分。
移动版浏览器引进了 `viewport` 这个 `meta tag`，让网页开发者来控制 `viewport` 的大小和缩放，其他手机浏览器也基本支持。

##### 例子
```
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1" />
```
##### content内容
![content内容](https://upload-images.jianshu.io/upload_images/1726248-dd37d922b0231f9e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 注意
如果不显示地设置viewport，那么width的默认为980。
如果页面的所有元素宽度都小于980，此时width为980，如果页面最宽的位置超过980，那么width等于最大宽度。总之，默认能将整个页面从左到右显示出来。
如果设置了`viewport`，比如，只单纯地设置了`user-scalable=no`，例如`<meta name="viewport" content="user-scalable=no" />`，那么ios下width还是按980显示（即默认就会通过dpi缩放），但android和winphone下却不会再缩放了，浏览器分辨率和真实设置分辨率一致。

### 三种视窗
##### 1、visual viewport（视觉视窗）
视觉视窗：就是设备的屏幕区域，换句话说就是用户通过屏幕所看到的页面内容。但它所对应的并不是指屏幕区域里的物理像素，而是CSS 像素。并且它所包含的 CSS 像素的数量也是随着用户缩放而有所改变。
![视觉窗口](https://upload-images.jianshu.io/upload_images/1726248-6697c843a3dbf82a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

`visual viewport` 的宽度可以通过 `document.documentElement.innerWidth` 来获取。

##### 2、layout viewport（布局视窗）
如果把移动设备上浏览器的可视区域设为 `viewport` 的话，某些网站会因为 `viewport` 太窄而显示错乱，所以这些浏览器就默认会把 `viewport` 设为一个较宽的值，比如980px，使得即使是那些为PC浏览器设计的网站也能在移动设备浏览器上正常显示。这个浏览器默认的 `viewport` 叫做 `layout viewport`。
![布局视窗](https://upload-images.jianshu.io/upload_images/1726248-596e1ba8c2e6889c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

`layout viewport` 的宽度可以通过 `document.documentElement.clientWidth` 来获取。

##### 3、ideal viewport（理想视窗）
`ideal viewport` 是一个能完美适配移动设备的 `viewport`。首先，不需要缩放和横向滚动条就能正常查看网站的所有内容；其次，显示的文字、图片大小合适，如14px的文字不会因为在一个高密度像素的屏幕里显示得太小而无法看清，无论是在何种密度屏幕，何种分辨率下，显示出来的大小都差不多。这个viewport叫做 `ideal viewport`。
`ideal viewport` 并没有一个固定的尺寸，不同的设备有不同的 `ideal viewport`。例如，所有的iphone的 `ideal viewport` 宽度都是320px，无论它的屏幕宽度是320还是640。
**`ideal viewport` 的意义在于，无论在何种分辨率的屏幕下，针对 `ideal viewport`  而设计的网站，不需要缩放和横向滚动条都可以完美地呈现给用户。**
