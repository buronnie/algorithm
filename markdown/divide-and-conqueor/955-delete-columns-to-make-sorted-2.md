这道题我一开始想复杂了，我想的是用分治的思想去解决这个问题。
* 对于每一列的字符进行分组，建立递推关系。
* 难点是前一组如果删除了某一列，那么之后的组应该跳过那一列的检查。
* 当前列开始往后的子串中，删除的列数等于每一组删除的列数之和。

```javascript
var minDeletionSize = function(A) {
    const len = A.length;
    if (len <= 1) {
        return 0;
    }
    const deletedLevels = new Set();
    return helper(A, 0, len-1, 0, deletedLevels);
};

function helper(A, start, end, level, deletedLevels) {
    if (level >= A[0].length) {
        return 0;
    }
    if (deletedLevels.has(level)) {
        return helper(A, start, end, level+1, deletedLevels);
    }
    if (start >= end) {
        return 0;
    }
    const blocks = [];
    let i = start;
    while (i <= end) {
        let j = i;
        while (i <= end-1) {
            if (A[i][level] === A[i+1][level]) {
                i++;
            } else if (A[i][level].charCodeAt(0) > A[i+1][level].charCodeAt(0)) {
                deletedLevels.add(level);
                const sum = helper(A, start, end, level, deletedLevels) + 1;
                return sum;
            } else {
                break;
            }
        }
        blocks.push([j, i]);
        i++;
    }
    let sum = 0;
    for (let i = 0; i < blocks.length; i++) {
        sum += helper(A, blocks[i][0], blocks[i][1], level+1, deletedLevels);
    }
    return sum;
}
```

另一种非常精妙的解法是建立boolean数组存储当前字符串是否已经是排序状态。
* 首先扫描一列，如果存在逆序，则删除这一列
* 如果没有逆序，则再扫描一遍，看看哪些字符串的顺序已经确定（除了字符相同的顺序不定）

```javascript
var minDeletionSize = function(A) {
    const len = A.length;
    if (len <= 1) {
        return 0;
    }
    const sorted = new Array(len);
    for (let i = 0; i< len; i++) {
        sorted[i] = false;
    }
    const levels = A[0].length;
    let res = 0;
    for (let l = 0; l < levels; l++) {
        let shouldDelete = false;
        for (let i = 0; i < len-1; i++) {
            if (!sorted[i] && A[i][l].charCodeAt(0) > A[i+1][l].charCodeAt(0)) {
                shouldDelete = true;
                res++;
                break;
            }
        }
        if (!shouldDelete) {
            for (let i = 0; i < len-1; i++) {
                if (A[i][l].charCodeAt(0) < A[i+1][l].charCodeAt(0)) {
                    sorted[i] = true;
                }
            }
        }
    }
    return res;
};
```