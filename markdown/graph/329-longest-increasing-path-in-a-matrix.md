这道题是伪hard，比有些medium还简单。用分治法不难得到 `d[i][j] = Math.max(d[i-1][j], d[i+1][j], d[i][j-1], d[i][j+1]]) + 1`
有点dp的意思，不过这道题不太好用dp的方法写，原因是通常dp[i][j]只依赖于之前访问过的节点，这里可以依赖还没有访问过的。
和dp很像的一类问题是memoized dfs，对于计算过的值可以记录下来，下次遇到就不用再计算一遍了。

```javascript
var longestIncreasingPath = function(matrix) {
    const rows = matrix.length;
    if (rows === 0) {
        return 0;
    }
    const cols = matrix[0].length;
    
    const map = {};
    let max = 0;
    for (let i = 0; i < rows; i++) {
        for (let j = 0; j < cols; j++) {
            max = Math.max(max, dfs(i, j, matrix, map));
        }
    }
    return max;
};

function dfs(x, y, matrix, map) {
    const code = `${x},${y}`;
    if (code in map) {
        return map[code];
    }
    const rows = matrix.length;
    const cols = matrix[0].length;
    const diff = [[-1, 0], [1, 0], [0, -1], [0, 1]];
    let max = 1;
    for (let d of diff) {
        if (x+d[0] >= 0 && x+d[0] < rows && y+d[1] >= 0 && y+d[1] < cols && matrix[x][y] < matrix[x+d[0]][y+d[1]]) {
            max = Math.max(max, 1 + dfs(x+d[0], y+d[1], matrix, map));
        }
    }
    map[code] = max;
    return max;
}
```