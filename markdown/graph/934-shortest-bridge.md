这道题和其它的islands题目非常类似。难点在于如何确定两个islands之间的最短距离。
最直接的想法是:
* dfs把两个islands的坐标分别记录下来，存在两个array中。
* 双重循环两个array, 找到其中Manhattan距离最短的两个点。
* 这种方法勉强AC, beat 0% (囧)

```javascript
var shortestBridge = function(grid) {
    const rows = grid.length;
    if (rows === 0) {
        return 0;
    }
    const cols = grid[0].length;

    const islands = [];
    for (let i = 0; i < rows; i++) {
        for (let j = 0; j < cols; j++) {
            if (grid[i][j] === 1) {
                islands.push(dfs(i, j, grid));
            }
        }
    }
    const land1 = islands[0];
    const land2 = islands[1];
    
    let res = rows + cols;
    for (let i = 0; i < land1.length; i++) {
        for (let j = 0; j < land2.length; j++) {
            res = Math.min(res,  dist(land1[i], land2[j]) - 1);
        }
    }
    return res;   
};

function dist(p1, p2) {
    return Math.abs(p1[0] - p2[0]) + Math.abs(p1[1] - p2[1]);
}


function dfs(row, col, grid) {
    const rows = grid.length;
    const cols = grid[0].length;
    grid[row][col] = 0;
    const dx = [0, 1, 0, -1];
    const dy = [1, 0, -1, 0];
    let arr = [[row, col]];
    for (let i = 0; i < 4; i++) {
        const nextRow = dx[i] + row;
        const nextCol = dy[i] + col;
        if (nextRow >= 0 && nextRow < rows && nextCol >= 0 && nextCol < cols && grid[nextRow][nextCol] === 1) {
            arr = [...arr, ...dfs(nextRow, nextCol, grid)];
        }
    }
    return arr;
}
```

更好的做法是：
* dfs找到一个island的所有点
* 用bfs从所有点向外扩张，直到碰到第二个island的点
  
```javascript
var shortestBridge = function(grid) {
    const rows = grid.length;
    if (rows === 0) {
        return 0;
    }
    const cols = grid[0].length;

    let queue;
    let flag = true;
    for (let i = 0; i < rows && flag; i++) {
        for (let j = 0; j < cols && flag; j++) {
            if (grid[i][j] === 1) {
                queue = dfs(i, j, grid);
                flag = false;
                break;
            }
        }
    }
    let dist = 0;
    const dx = [0, 1, 0, -1];
    const dy = [1, 0, -1, 0];
    while (queue.length > 0) {
        const size = queue.length;
        for (let i = 0; i < size; i++) {
            const [x, y] = queue.pop();
            grid[x][y] = -1;
            for (let j = 0; j < 4; j++) {
                if (x+dx[j] >= 0 && x+dx[j] < rows && y+dy[j] >= 0 && y+dy[j] < cols && grid[x+dx[j]][y+dy[j]] !== -1) {
                    if (grid[x+dx[j]][y+dy[j]] === 1) {
                        return dist;
                    }
                    queue.unshift([x+dx[j], y+dy[j]]);
                }
            }
        }
        dist++;
    }
    return dist;    
};

function dfs(row, col, grid) {
    const rows = grid.length;
    const cols = grid[0].length;
    grid[row][col] = 0;
    const dx = [0, 1, 0, -1];
    const dy = [1, 0, -1, 0];
    let arr = [[row, col]];
    for (let i = 0; i < 4; i++) {
        const nextRow = dx[i] + row;
        const nextCol = dy[i] + col;
        if (nextRow >= 0 && nextRow < rows && nextCol >= 0 && nextCol < cols && grid[nextRow][nextCol] === 1) {
            arr = [...arr, ...dfs(nextRow, nextCol, grid)];
        }
    }
    return arr;
}
```