这道题和200题number of islands很像，唯一的区别就是要检查island的形状。难点在于如何给形状编码。因为每次dfs traverse的路径是相同的，可以设每一个island的起点为归一化的零点，然后把沿路访问过的归一化的节点坐标记录下来, 就可以做为island的形状编码。

```javascript
var numDistinctIslands = function(grid) {
    const rows = grid.length;
    if (rows === 0) {
        return 0;
    }
    const cols = grid[0].length;
    
    const set = new Set();
    let res = 0;
    for (let i = 0; i < rows; i++) {
        for (let j = 0; j < cols; j++) {
            if (grid[i][j] === 1) {
                const code = dfs(i, j, grid, 0, 0);
                if (!set.has(code)) {
                    res++;
                    set.add(code);
                }
            }
        }
    }
    return res;
};

function dfs(row, col, grid, row0, col0) {
    const rows = grid.length;
    const cols = grid[0].length;
    let code = `${row0}${col0}`;
    const dx = [1, 0, -1, 0];
    const dy = [0, 1, 0, -1];
    grid[row][col] = 0;
    for (let i = 0; i < 4; i++) {
        if (row+dx[i] >= 0 && row+dx[i] < rows && col+dy[i] >= 0 && col+dy[i] < cols && grid[row+dx[i]][col+dy[i]] === 1) {
            code += dfs(row+dx[i], col+dy[i], grid, row0+dx[i], col0+dy[i]);
        }
    }
    return code;
}
```