这道题和200，694非常类似，关键在于计算出每个islands的节点个数。

```javascript
var maxAreaOfIsland = function(grid) {
    const rows = grid.length;
    if (rows === 0) {
        return 0;
    }
    const cols = grid[0].length;
    
    const set = new Set();
    let max = 0;
    for (let i = 0; i < rows; i++) {
        for (let j = 0; j < cols; j++) {
            if (grid[i][j] === 1) {
                max = Math.max(max, dfs(i, j, grid));
            }
        }
    }
    return max;
};

function dfs(row, col, grid) {
    const rows = grid.length;
    const cols = grid[0].length;
    const dx = [1, 0, -1, 0];
    const dy = [0, 1, 0, -1];
    grid[row][col] = 0;
    let sum = 1;
    for (let i = 0; i < 4; i++) {
        if (row+dx[i] >= 0 && row+dx[i] < rows && col+dy[i] >= 0 && col+dy[i] < cols && grid[row+dx[i]][col+dy[i]] === 1) {
            sum += dfs(row+dx[i], col+dy[i], grid);
        }
    }
    return sum;
}
```