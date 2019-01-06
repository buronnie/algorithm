一般看到maximum/longest subarray问题首先想到presum。这道题我们可以将所有的presum缓存起来，然后看看是否当前的presum-k已经存在。trick是presum=0的时候，index=-1。

```javascript
var maxSubArrayLen = function(nums, k) {
    const len = nums.length;
    if (len === 0) {
        return 0;
    }
    const map = {0: -1};
    let presum = 0;
    let res = 0;
    for (let i = 0; i < len; i++) {
        presum += nums[i];
        const left = map[presum - k];
        if (left !== undefined) {
            res = Math.max(res, i - left);
        }
        if (!(presum in map)) {
            map[presum] = i;
        }
    }
    return res;
};
```