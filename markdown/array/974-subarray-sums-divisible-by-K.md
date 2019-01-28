这道题一看到sumarray sum就应该想到presum，如果暴力求解是O(n^2)的复杂度，会超时。如果我们能在计算presum的同时，记录下每个余数出现的次数，那么presum[i]的余数就可以帮助我们知道divisble by K的数目。例如presum[i] % K = 3, 之前余数为3的presum出现了2次，那么以当前i结尾的subarray中，能够被K整除的subarray数目就是2次。需要注意是当presum[i] < 0时，需要先加上若干个K使其先变成正数，然后再求余数。

```javascript
var subarraysDivByK = function(A, K) {
    const len = A.length;
    if (len === 0) {
        return 0;
    }
    const presum = new Array(len+1);
    presum[0] = 0;
    const modMap = {0: 1};
    let res = 0;
    for (let i = 1; i <= len; i++) {
        presum[i] = presum[i-1] + A[i-1];
        let mod = presum[i] % K;
        if (presum[i] < 0) {
            mod = ((Math.floor(-presum[i] / K) + 1) * K + presum[i]) % K;
        }
        if (!(mod in modMap)) {
            modMap[mod] = 1;
        } else {
            res += modMap[mod];
            modMap[mod]++;
        }
    }
    return res;
};
```