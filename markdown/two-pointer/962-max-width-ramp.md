这道题乍一看没有什么思路，似乎只能用双重循环暴力求解。但是仔细想想发现是一道双指针滑动窗口题。我们需要维护一个滑动的窗口，使得窗口的大小尽量最大。

其中还需要一个辅助的array，作用是记录从当前位置到最右边这个子序列的最大值。

一开始两个指针都在最左边，首先试图尽量向右移动右边的指针。当右边子序列有数比左指针的数大时，我们就可以一直移动右指针。当右指针所在的右序列没有数比左指针的数大时，开始移动左指针，直到右指针所在的右序列有数比左指针的数大停止。注意要检查右指针不能越界。

```javascript
var maxWidthRamp = function(A) {
    const len = A.length;
    if (len <= 1) {
        return 0;
    }
    const max = new Array(len);
    max[len-1] = A[len-1];
    for (let i = len-2; i >= 0; i--) {
        max[i] = Math.max(max[i+1], A[i]);
    }
    
    let left = 0, right = 0, res = 0;
    while (right < len) {
        while (right < len && A[left] <= max[right]) {
            res = Math.max(res, right-left);
            right++;
        }
        if (right === len) {
            return res;
        }
        while (A[left] > max[right]) {
            left++;
        }
    }
    return res;
};
```