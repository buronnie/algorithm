Palindrome问题常见的方法有：
* DP: 二维dp[start][len]记录每个子串是否是palindrome
* 扩展法：从一个letter开始左右扩展

DP: 

```javascript
var countSubstrings = function(s) {
    const len = s.length;
    if (len === 0) {
        return 0;
    }
    if (len === 1) {
        return 1;
    }
    const dp = new Array(len);
    for (let i = 0; i< len; i++) {
        dp[i] = new Array(len);
        for (let j = 0; j < len; j++) {
            dp[i][j] = false;
        }
    }
    
    let res = 0;
    for (let i = 0; i < len; i++) {
        dp[i][0] = true;
        res++;
    }
    for (let i = 0; i < len-1; i++) {
        if (s[i] === s[i+1]) {
            dp[i][1] = true;
            res++;
        }
    }
    for (let gap = 2; gap < len; gap++) {
        for (let i = 0; i+gap < len; i++) {
            if (s[i] === s[i+gap] && dp[i+1][gap-2]) {
                dp[i][gap] = true;
                res++;
            }
        }
    }
    return res;
};
```

扩展法：

```javascript
var countSubstrings = function(s) {
    const len = s.length;
    if (len === 0) {
        return 0;
    }
    if (len === 1) {
        return 1;
    }
    let res = 0;
    for (let i = 0; i < len; i++) {
        res += extend(s, i, i);
        res += extend(s, i, i+1);
    }
    return res;
};

function extend(s, left, right) {
    const len = s.length;
    let count = 0;
    while (left >= 0 && right < len) {
        if (s[left] === s[right]) {
            count++;
            left--;
            right++;
        } else {
            break;
        }
    }
    return count;
}
```