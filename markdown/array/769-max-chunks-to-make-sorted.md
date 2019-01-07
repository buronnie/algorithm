这道题关键是找到partition的条件，那就是对于index i，左边subarray的最大值 <= 右边subarray的最小值。
可以维护maxLeft, minRight array，使判断简化。

```javascript
var maxChunksToSorted = function(arr) {
    const len = arr.length;
    if (len === 0) {
        return 0;
    }
    
    const maxLeft = new Array(len);
    const minRight = new Array(len);
    maxLeft[0] = arr[0];
    minRight[len-1] = arr[len-1];
    for (let i = 1; i < len; i++) {
        maxLeft[i] = Math.max(maxLeft[i-1], arr[i]);
    }
    for (let i = len-2; i>= 0; i--) {
        minRight[i] = Math.min(minRight[i+1], arr[i]);
    }
    return helper(arr, maxLeft, minRight, 0, len-1);
};

function helper(arr, maxLeft, minRight, left, right) {
    if (left > right) {
        return 0;
    }

    for (let i = left; i < right; i++) {
        if (maxLeft[i] <= minRight[i+1]) {
            return helper(arr, maxLeft, minRight, left, i) + helper(arr, maxLeft, minRight, i+1, right) ;
        }
    }
    return 1;
}
```