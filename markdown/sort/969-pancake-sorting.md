这道题描述的比较复杂，其实就是利用不停的反转来实现排序。每一次找到最大数，把它翻转到头，再翻转到尾，重复N次即可。

```javascript
var pancakeSort = function(A) {
    const len = A.length;
    if (len <= 1) {
        return [];
    }
    const res = [];
    let maxIndex = 0;
    for (let i = 0; i < len; i++) {
        if (A[i] >= A[maxIndex]) {
            maxIndex = i;
        }
    }
    if (maxIndex < len-1) {
        if (maxIndex > 0) {
            res.push(maxIndex+1);
            reverse(A, 0, maxIndex);
        }
        res.push(len);
        reverse(A, 0, len-1);
    }
    return res.concat(pancakeSort(A.slice(0, len-1)));
};

function reverse(A, start, end) {
    while (start < end) {
        swap(A, start++, end--);
    }
}

function swap(A, i, j) {
    const tmp = A[i];
    A[i] = A[j];
    A[j] = tmp;
}
```