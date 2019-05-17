看到consecutive sum就应该想到这是一个DP问题。当我们知道dp[0], dp[1], ...dp[i-1]时，如何推出dp[i]呢？
A[i]的效果是有可能影响到它前面最多K个数，有K种可能得到最大值：

A[i]单独是一个partition, dp[i] = dp[i-1] + A[i]

A[i], A[i-1]是一个partition, dp[i] = dp[i-2] + max(A[i], A[i-1]) * 2

...

A[i], A[i-1], ..., A[i-j]是一个partition, dp[i] = dp[i-j-1] + max(A[i], A[i-1], ..., A[i-j]) * (j+1)

```
class Solution {
    public int maxSumAfterPartitioning(int[] A, int K) {
        int len = A.length;
        if (len == 0) {
            return 0;
        }
        if (len == 1) {
            return A[0];
        }
        int[] dp = new int[len];
        dp[0] = A[0];
        int res = Integer.MIN_VALUE;
        for (int i = 1; i < len; i++) {
            int max = Integer.MIN_VALUE;
            dp[i] = Integer.MIN_VALUE;
            for (int j = 0; j < K; j++) {
                if (i-j < 0) {
                    break;
                }
                max = Math.max(max, A[i-j]);
                if (i-j-1 == -1) {
                    dp[i] = Math.max(dp[i], max*(j+1));
                } else {
                    dp[i] = Math.max(dp[i], dp[i-j-1] + max*(j+1));
                }
            }
            res = Math.max(res, dp[i]);
        }
        return res;
    }
}
```