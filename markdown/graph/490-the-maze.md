这道题乍一看有点难，其实还是bfs的变种。不同于一般的island问题（找四个邻接点), 这里的neighbor是沿一个方向一直走下去直到走不动。在处理完当前节点后可以把它标记为-1，以避免回头的问题。

```javascript
var hasPath = function(maze, start, dest) {
    const rows = maze.length;
    if (rows === 0) {
        return true;
    }
    if (maze[start[0]][start[1]] === 1 || maze[dest[0]][dest[1]] === 1) {
        return false;
    }
    const cols = maze[0].length;
    
    const queue = [start];
    while (queue.length > 0) {
        const [x, y] = queue.pop();
        if (x === dest[0] && y === dest[1]) {
            return true;
        }
        // search left
        let i = y;
        while (i >= 0 && maze[x][i] === 0) {
            i--;
        }
        if (i+1 !== y) {
            if (i < 0 || maze[x][i] === 1) {
                queue.unshift([x, i+1]);
            }
        }
        
        // search right
        i = y;
        while (i < cols && maze[x][i] === 0) {
            i++;
        }
        if (i-1 !== y) {
            if (i > cols-1 || maze[x][i] === 1) {
                queue.unshift([x, i-1]);
            }
        }
        
        // search up
        i = x;
        while (i >= 0 && maze[i][y] === 0) {
            i--;
        }
        if (i+1 !== x) {
            if (i < 0 || maze[i][y] === 1) {
                queue.unshift([i+1, y]);
            }
        }
        
        // search down
        i = x;
        while (i < rows && maze[i][y] === 0) {
            i++;
        }
        if (i-1 !== x) {
            if (i > rows - 1 || maze[i][y] === 1) {
                queue.unshift([i-1, y]);
            }
        }
        maze[x][y] = -1;
    }
    return false;
};
```