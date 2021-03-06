### 时间复杂度

#### 1、算法的效率
算法的效率主要由以下两个复杂度来评估：
**时间复杂度：**评估执行程序所需的时间。可以估算出程序对处理器的使用程度。
**空间复杂度：**评估执行程序所需的存储空间。可以估算出程序对计算机内存的使用程度。

#### 2、时间复杂度
**时间频度**
一个算法执行所耗费的时间，从理论上是不能算出来的，必须上机运行测试才能知道。但我们不可能也没有必要对每个算法都上机测试，只需知道哪个算法花费的时间多，哪个算法花费的时间少就可以了。并且一个算法花费的时间与算法中语句的执行次数成正比例，哪个算法中语句执行次数多，它花费时间就多。一个算法中的语句执行次数称为语句频度或时间频度。记为 `T(n)`。

**时间复杂度**
前面提到的时间频度 `T(n)` 中，n称为问题的规模，当n不断变化时，时间频度 `T(n)` 也会不断变化。但有时我们想知道它变化时呈现什么规律，为此我们引入时间复杂度的概念。一般情况下，算法中基本操作重复执行的次数是问题规模n的某个函数，用 `T(n)` 表示，若有某个辅助函数 `f(n)`，使得当n趋近于无穷大时，`T(n)/f(n)` 的极限值为不等于零的常数，则称 `f(n)` 是 `T(n)` 的同数量级函数，记作 `T(n)=O(f(n))`，它称为算法的渐进时间复杂度，简称时间复杂度。

#### 3、大O表示法
**用O(n)来体现算法时间复杂度的记法被称作大O表示法。**
一般我们我们评估一个算法都是直接评估它的最坏的复杂度。

大O表示法 `O(f(n))` 中的 `f(n)` 的值可以为 `1`、`n`、`logn`、`n^2` 等，所以我们将 `O(1)`、`O(n)`、`O(logn)`、`O( n^2 )` 分别称为**常数阶、线性阶、对数阶和平方阶**。下面我们来看看推导大O阶的方法：

**推导大O阶**
推导大O阶有一下三种规则：
- 1、用常数1来取代运行时间中所有加法常数
- 2、修改后的运行次数函数中，只保留最高阶项
- 3、如果最高阶项存在且不是1，则去除与这个项相乘的常数

##### 常数阶
see the code:
```javascript
let sum = 0, n = 100;   //执行一次
sum = (1+n)*n/2;   //执行一次
console.log( `The sum is : ${sum}` );   //语句执行一次
```
上面算法的运行的次数的函数为 `f(n)=3`，根据推导大O阶的**规则1**，我们需要**将常数3改为1**，则这个算法的时间复杂度为 `O(1)`。如果 `sum = (1+n)*n/2` 这条语句再执行10遍，因为这与问题大小n的值并没有关系，所以这个算法的时间复杂度仍旧是O(1)，我们可以称之为**常数阶**。

##### 线性阶
see the code:
```javascript
let i =0; // 语句执行一次
while ( i < n )  { // 语句执行n次
  console.log( `Current i is ${i}` ); //语句执行n次
  i++; // 语句执行n次
}
```
上面算法循环体中的代码执行了n次，因此时间复杂度为 `O(n)`。

##### 对数阶
see the code:
```javascript
let number = 1; // 语句执行一次
while ( number < n )  {   // 语句执行logn次
    number *= 2;             // 语句执行logn次
}
```
上面算法中，随着 `number` 每次乘以2后，都会越来越接近 `n`，当 `number` 不小于 `n` 时就会退出循环。假设循环的次数为 `x`，则由 `2^x = n` 得出 `x = log₂n`，因此得出这个算法的时间复杂度为 `O(logn)`。

##### 平方阶
see the code:
```javascript
for (let i = 0; i < n; i++) { // 语句执行n次
  for (let j = 0; j < n; j++) { // 语句执行n^2次
     console.log( 'code...' ); // 语句执行n^2
  }
}
```
上面的嵌套循环中，代码共执行 `2*n^2 + n`，则 `f(n) = n^2`。所以该算法的时间复杂度为 `O(n^2 )`。

**算一下下面算法的时间复杂度：**
```javascript
  for(let i = 0; i < n; i++){
      for(let j = i; j < n; j++){
         // code...
         ...
      }
  }
```
需要注意的是内循环中 `let j = i`，而不是 `let j = 0`。当 `i = 0` 时，内循环执行了n次；`i = 1` 时内循环执行了 `n - 1` 次，当 `i = n - 1` 时执行了1次，我们可以推算出总的执行次数为：
```javascript
n+(n-1)+(n-2)+(n-3)+……+1
=(n+1)+[(n-1)+2]+[(n-2)+3]+[(n-3)+4]+……
=(n+1)+(n+1)+(n+1)+(n+1)+……
=(n+1)n/2
=n(n+1)/2
=n²/2+n/2
```

根据此前讲过的推导大O阶的规则的第二条：只保留最高阶，因此保留 `n²/2`。根据第三条去掉和这个项的常数，则去掉 `1/2`，最终这段代码的时间复杂度为 `O(n²)`。

### 4、常见时间复杂度的比较
常用的时间复杂度按照耗费的时间从小到大依次是：
```javascript
O(1)<O(logn)<O(n)<O(nlogn)<O(n²)<O(n³)<O(2ⁿ)<O(n!)
```
