# 选择排序
**选择排序规则：**
- 选择排序大致的思路是找到数据结构中的最小值并将其放置在第一位，接着找到第二小的值并将其放在第二位，以此类推。

**selectionSort.js**
```javascript
class SelectionSort {
    constructor( arr ) {
        this.array = arr || [];
    }

    insert( item ) {
        this.array.push( item );
    }

    toString() {
        return this.array.join();
    }

    selectionSort() {
        let len = this.array.length;
        let min = "";

        for( let i = 0; i < len - 1; i++ ) {
            min = i;
            for( let j = i; j < len; j++ ) {
                if( this.array[min] > this.array[j] ) {
                    min = j;
                }
            }

            if( i !== min ) {
                this._swap( this.array, i, min );
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
        <title>选择排序</title>
    </head>
    <body>

        <script type="text/javascript" src="selectionSort.js"></script>
        <script type="text/javascript">
            // function createNonSortedArray( size ) {
            //     let array = new SelectionSort();
            //     for( let i = size; i > 0; i-- ) {
            //         array.insert( i );
            //     }
            //     return array;
            // }
            //
            // let array = createNonSortedArray( 7 );

            let array = new SelectionSort( [1, 5, 2, 12, 8, 0] );
            console.log( array.toString() );
            array.selectionSort();
            console.log( "selectionSort:", array.toString() );
        </script>
    </body>
</html>
```
**测试结果：**

![选择排序](https://upload-images.jianshu.io/upload_images/1726248-781cf94fddada7bc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
