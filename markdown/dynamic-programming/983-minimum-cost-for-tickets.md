这道题还是比较明显的DP问题，在每一个旅行天上有三种选择：
1) 买1-day pass，那么要这一天为止最小总花销就是dp[i-1] + costs[0]
2) 买7-day pass, 那么就要往前推一直找到距离今天超过7天的旅行日子，最小总花销就是dp[j] + costs[1] where argmax(j, days[i] - days[j] > 6)
2) 买30-day pass, 那么就要往前推一直找到距离今天超过30天的旅行日子，最小总花销就是dp[j] + costs[2] where argmax(j, days[i] - days[j] > 29)

```javascript
var mincostTickets = function(days, costs) {
    const len = days.length;
    if (len === 0) {
        return 0;
    }
    const dp = new Array(len);
    dp[0] = costs[0];
    
    for (let i = 1; i < len; i++) {
        const val1 = dp[i-1] + costs[0];
        let j = i-1;
        while (j >= 0 && days[i] - days[j] <= 6) {
            j--;
        }
        let val2 = j < 0 ? costs[1] : dp[j] + costs[1];
        j = i-1;
        while (j >= 0 && days[i] - days[j] <= 29) {
            j--;
        }
        let val3 = j < 0 ? costs[2] : dp[j] + costs[2];
        dp[i] = Math.min(val1, val2, val3);
    }
    return dp[len-1];
};
```