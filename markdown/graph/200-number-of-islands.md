这道题和947题很类似，本质上是图的连通性问题，利用dfs遍历所有连通在一起的点。这个题也有自己的特殊性，使代码简化：
* 不需要额外定义visited矩阵，可以直接修改 `grid[i][j] = 0` 来判断一个点是否已经访问过
* 每一个起点是一个新的island的开始，dfs函数不需要返回任何值，在主函数中设计数器即可

```javascript
var numIslands = function(grid) {
    const rows = grid.length;
    if (rows === 0) {
        return 0;
    }
    const cols = grid[0].length;

    let res = 0;
    for (let i = 0; i < rows; i++) {
        for (let j = 0; j < cols; j++) {
            if (grid[i][j] === '1') {
                dfs(i, j, grid);
                res += 1;
            }
        }
    }
    return res;
};


function dfs(row, col, grid) {
    const rows = grid.length;
    const cols = grid[0].length;
    grid[row][col] = '0';
    const dx = [0, 1, 0, -1];
    const dy = [1, 0, -1, 0];
    for (let i = 0; i < 4; i++) {
        const nextRow = dx[i] + row;
        const nextCol = dy[i] + col;
        if (nextRow >= 0 && nextRow < rows && nextCol >= 0 && nextCol < cols && grid[nextRow][nextCol] === '1') {
            dfs(nextRow, nextCol, grid);
        }
    }
}
```
