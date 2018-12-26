这首题一开始想到dp, 分治去了，因为有点像取石子的题。用分治法写很麻烦，最后还超时了。没办法重新想思路，其实这道题有一个隐含条件，那就是买入最小的，卖出最大的，可以凭空增加power的数量，然后再检查是否能买到更多的分数。本质上还是一道双指针题。要注意每次买入前要检察是否当前的power数足够买入point.

```javascript
var bagOfTokensScore = function(tokens, P) {
    const len = tokens.length;
    if (len === 0 || P <= 0) {
        return 0;
    }
    tokens.sort((a, b) => a - b);
    
    let max = 0;
    let start = 0, end = len-1;
    while (start <= end) {
        max = Math.max(max, getPoints(tokens, start, end, P));
        if (P >= tokens[start]) {
            P += (tokens[end--] - tokens[start++]);
        } else {
            break;
        }
    }
    return max;
};

function getPoints(tokens, start, end, P) {
    if (end - start < 0) {
        return 0;
    }
    let points = 0;
    while (start <= end && P >= tokens[start]) {
        P -= tokens[start++];
        points++;
    }
    return points;
}
```