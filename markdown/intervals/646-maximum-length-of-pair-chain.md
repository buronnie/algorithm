这道题本质上是一个interval融合问题。一般我们解决interval问题都是按左边界排序，但是这一道题用右边界排序会更简单。用右边界排序的好处是当第i个和第i+1个interval有重叠的时候，可以果断放弃第i+1个，而不会担心是否错过最优解，因为能和第i+1个融合的interval也必然能和第i个融合。

```javascript
var findLongestChain = function(pairs) {
    const len = pairs.length;
    if (len <= 1) {
        return len;
    }
    pairs.sort((a, b) => a[1] - b[1]);
    let res = 1;
    let curr = pairs[0];
    let i = 1;
    while (i < len) {
        const next = pairs[i];
        if (curr[1] < next[0]) {
            res++;
            curr = [Math.min(curr[0], next[0]), next[1]];
        }
        i++;
    }
    return res;
};
```