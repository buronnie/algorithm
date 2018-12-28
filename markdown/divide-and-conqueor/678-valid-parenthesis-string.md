这道题比没有wild card的那道看起来要难了不少。关键是每次遇到*就会有3个分支，这样总共的可能性是指数上升的。不考虑复杂度的问题，最直接的想法是分治。用一个counter统计左右括号出现次数的差。合法的字符串要求最终的counter = 0。

```javascript
var checkValidString = function(s) {
    const len = s.length;
    if (len === 0) {
        return true;
    }
    return dfs(0, s, 0);
};

function dfs(pos, str, curr) {
    if (pos >= str.length) {
        return curr === 0;
    }
    if (curr < 0) {
        return false;
    }
    if (str[pos] === '(') {
        return dfs(pos+1, str, curr+1);
    } else if (str[pos] === ')') {
        return dfs(pos+1, str, curr-1);
    }
    return dfs(pos+1, str, curr) || dfs(pos+1, str, curr+1) || dfs(pos+1, str, curr-1);
}
```