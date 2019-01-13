# 冒泡排序
**冒泡排序规则：**
- 比较相邻的元素。如果第一个比第二个大，就交换他们两个；
- 对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对。这步做完后，最后的元素会是最大的数；
- 持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较。

**bubbleSort.js**
```javascript
class BubbleSort {
    constructor() {
        this.array = [];
    }

    insert( item ) {
        this.array.push( item );
    }

    toString() {
        return this.array.join();
    }

    bubbleSort() {
        let len = this.array.length;
        for( let i = 0; i < len; i++ ) {
            for( let j = 0; j < len - 1; j++ ) {
                if( this.array[j] > this.array[j + 1] ) {
                    this._swap( this.array, j, j + 1 );
                }
            }
        }
    }

    // 改进后的冒泡排序
    // 如果从内循环减去外循环中已跑过的轮数，就可以避免内循环中所有不必要的比较
    modifiedBubbleSort() {
        let len = this.array.length;
        for( let i = 0; i < len; i++ ) {
            for( let j = 0; j < len - 1 - i; j++ ) {
                if( this.array[j] > this.array[j + 1] ) {
                    this._swap( this.array, j, j + 1 );
                }
            }
        }
    }

    _swap( array, i, j ) {
        [array[i], array[j]] = [array[j], array[i]];
    }
}
```

**测试页面代码：**
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>冒泡排序</title>
    </head>
    <body>

        <script type="text/javascript" src="bubbleSort.js"></script>
        <script type="text/javascript">
            function createNonSortedArray( size ) { //{6}
                let array = new BubbleSort();
                for( let i = size; i > 0; i-- ) {
                    array.insert( i );
                }
                return array;
            }

            let array = createNonSortedArray( 5 ); //{7}
            console.log( array.toString() );
            array.bubbleSort();
            console.log( "bubbleSort:", array.toString() );
            array.bubbleSort();
            console.log( "modifiedBubbleSort:", array.toString() );
        </script>
    </body>
</html>
```

**测试结果：**
![冒泡排序](https://upload-images.jianshu.io/upload_images/1726248-919a7dd046133941.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
