这道题一看就是无脑dfs，结果超时。。。。果然这题没想像中简单。
还是贴出来dfs吧
```javascript
var change = function(amount, coins) {
    const len = coins.length;
    if (len === 0) {
        if (amount === 0) {
            return 1;
        }
        return 0;
    }
    const res = [0];
    helper(coins, 0, 0, amount, res);
    return res[0];
};

function helper(coins, pos, curr, amount, res) {
    if (amount === curr) {
        res[0]++;
        return;
    }
    if (curr > amount) {
        return;
    }
    for (let i = pos; i < coins.length; i++) {
        helper(coins, i, curr + coins[i], amount, res);
    }
}
```

其实这也是一道经典的背包问题，略有不同的是这里面的元素可以被多次使用。注意初始状态的选择
```javascript
var change = function(amount, coins) {
    const len = coins.length;
    if (len === 0) {
        if (amount === 0) {
            return 1;
        }
        return 0;
    }
    const dp = new Array(len+1);
    for (let i = 0; i <= len; i++) {
        dp[i] = new Array(amount+1).fill(0);
    }
    dp[0][0] = 1;
    
    for (let i = 1; i <= len; i++) {
        dp[i][0] = 1;
        for (let j = 1; j <= amount; j++) {
            dp[i][j] = dp[i-1][j];
            if (j >= coins[i-1]) {
                // 多次使用
                dp[i][j] += dp[i][j-coins[i-1]];
                // 每个元素只能使用一次
                // dp[i][j] += dp[i-1][j-coins[i-1]];
            }
        }
    }
    return dp[len][amount];
};
```
