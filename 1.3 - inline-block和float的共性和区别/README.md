# inline-block和float的共性和区别
### 共性：
 - `inline-block`: 是把一个元素的display设置为**块状内联元素**，意思就是说，让一个元素的容器inline展示，并且里面的内容block展示；`inline`属性使元素内联展示，内联元素设置宽度无效，相邻的inline元素会在一行显示不换行，直到本行排满为止。`block`的元素始终会独占一行，呈块状显示，可设置宽高。所以`inline-block`的元素就是宽高可设置，相邻的元素会在一行显示，直到本行排满，也就是让元素的容器属性为block，内容为inline。

- `float`： 设置元素的浮动为左或者右浮动，当设置元素浮动时，相邻元素会根据自身大小，排满一行，如果父容器宽度不够则会换行。当我们设置了元素的浮动时，这个元素就脱离了文档流，相邻元素会呈环绕装排列。

**两者共同点是都可以实现元素在一行显示，并且可以自由设置元素大小。**

### 区别：
1、`inline-block` 会让元素有间距空白，`float` 则不会，如下图：
![区别](https://upload-images.jianshu.io/upload_images/1726248-67cf5d15635ef558.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2、排列问题
- `inline-block`: 水平排列一行，即使元素高度不一，也会以高度最大的元素高度为行高，即使高度小的元素周围留空，也不回有第二行元素上浮补位。可以设置默认的垂直对齐基线。

- `float`: 让元素脱离当前文档流，呈环绕装排列，如遇上行有空白，而当前元素大小可以挤进去，这个元素会在上行补位排列。默认是顶部对齐。
![image.png](https://upload-images.jianshu.io/upload_images/1726248-3bd8f530e7291177.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
