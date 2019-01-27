这道题是一道典型的dfs问题，在构造出满足条件的数之后将其存入结果数组中。

```javascript
var numsSameConsecDiff = function(N, K) {
    const res = [];
    if (N === 0) {
        return res;
    }
    for (let i = N === 1 ? 0 : 1; i <= 9; i++) {
        helper(N, K, 0, [i], res);
    }
    return res;
};

function helper(N, K, pos, curr, res) {
    if (pos === N-1) {
        res.push(curr.join(''));
        return;
    }
    
    if (curr[curr.length-1] + K <= 9) {
        curr.push(curr[curr.length-1] + K);
        helper(N, K, pos+1, curr, res);
        curr.pop();
    }
    if (K !== 0 && curr[curr.length-1] - K >= 0) {
        curr.push(curr[curr.length-1] - K);
        helper(N, K, pos+1, curr, res);
        curr.pop();
    }
}
```