这个题虽然是hard，但是思路还是非常直接的，就是代码量大一点，一次AC有难度。基本思路是
* 扫描array，记录树的高度，并从小到大排序
* 建立树的高度与树的坐标之间的map，方便根据排序的树高找到树的位置
* 遍历高度array, 知道当前和下一颗树的位置，用bfs找到最短距离。如果无法到达，则返回-1.

```javascript
var cutOffTree = function(forest) {
    const rows = forest.length;
    if (rows === 0) {
        return 0;
    }
    const cols = forest[0].length;
    if (forest[0][0] === 0) {
        return -1;
    }
    
    const map = {};
    const arr = [];
    for (let i = 0; i < rows; i++) {
        for (let j = 0; j < cols; j++) {
            if (forest[i][j] > 1) {
                map[forest[i][j]] = [i, j];
                arr.push(forest[i][j]);
            }
        }
    }
    
    arr.sort((a, b) => a - b);
    let res = 0;
    let start = [0, 0];
    for (let i = 0; i < arr.length; i++) {
        const dist = nearestPath(forest, start, map[arr[i]]);
        if (dist === -1) {
            return -1;
        }
        res += dist;
        start = map[arr[i]];
    }
    return res;
};

function nearestPath(forest, start, end) {
    const rows = forest.length;
    const cols = forest[0].length;
    const visited = new Array(rows);
    for (let i = 0; i < rows; i++) {
        visited[i] = new Array(cols);
        for (let j = 0; j < cols; j++) {
            visited[i][j] = false;
        }
    }
    const queue = [start];
    visited[start[0]][start[1]] = true;
    let level = 0;
    while (queue.length > 0) {
        const size = queue.length;
        for (let i = 0; i < size; i++) {
            const curr = queue.pop();
            if (curr[0] === end[0] && curr[1] === end[1]) {
                return level;
            }
            const diff = [[0, -1], [0, 1], [1, 0], [-1, 0]];
            for (let j = 0; j < diff.length; j++) {
                const nextRow = curr[0]+diff[j][0];
                const nextCol = curr[1]+diff[j][1];
                if (isValidPos(nextRow, nextCol, rows, cols, visited, forest)) {
                    visited[nextRow][nextCol] = true;
                    queue.unshift([nextRow, nextCol]);
                }
            }
        }
        level++;
    }
    return -1;
}

function isValidPos(x, y, rows, cols, visited, forest) {
    return x >= 0 && x < rows &&
           y >= 0 && y < cols &&
           forest[x][y] > 0 &&
           !visited[x][y];
}
```