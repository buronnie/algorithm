这道题需要按绝对值排序，从小到大顺藤摸瓜。
* 用hashmap记录每个数出现的次数
* 绝对值排序序列
* 从左向右扫描序列，遇到n, 2n就在hashmap中找到相应的数并减一, counter加二
* 看看counter是不是等于序列的长度

```javascript
var canReorderDoubled = function(A) {
    const len = A.length;
    if (len === 0) {
        return true;
    }
    A.sort((a, b) => Math.abs(a) - Math.abs(b));
    const map = {};
    for (let i = 0; i < len; i++) {
        if (A[i] in map) {
            map[A[i]]++;
        } else {
            map[A[i]] = 1;
        }
    }
    let count = 0;
    for (let i = 0; i < len; i++) {
        if (map[A[i]] > 0) {
            map[A[i]]--;
            const double = 2*A[i];
            if (!(double in map) || map[double] === 0) {
                return false;
            }
            map[double]--;
            count += 2;
        }
    }
    return count === len;
    
};
```