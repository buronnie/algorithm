这道题本质上是图的连通性问题。通过行列坐标可以构造出无向图。然后DFS遍历每一个节点，数算出图中所有连通的节点数。需要注意的是每次出发的起点不要计算，因为删除到最后每个连通子图需要留下至少一个点。这个方法是通用的，不论在什么规则下导致两点连通，我们都可以用这种方法先构造出adjacent map。下次即使对角线上的点也连通，也可以用这种方法。

```javascript
var removeStones = function(stones) {
    const len = stones.length;
    if (len <= 1) {
        return 0;
    }
    const map = {};
    for (let i = 0; i< len; i++) {
        for (let j = i+1; j < len; j++) {
            if (stones[i][0] === stones[j][0] || stones[i][1] === stones[j][1]) {
                if (i in map) {
                    map[i].push(j);
                } else {
                    map[i] = [j];
                }
                if (j in map) {
                    map[j].push(i);
                } else {
                    map[j] = [i];
                }
            }
        }
    }
    
    const visited = new Array(len);
    for (let i = 0; i < len; i++) {
        visited[i] = false;
    }
    
    let res = 0;
    for (let i = 0; i < len; i++) {
        res += dfs(i, visited, map, true);
    }
    return res;
};

function dfs(curr, visited, map, isStart) {
    if (visited[curr]) {
        return 0;
    }
    if (!(curr in map)) {
        return 0;
    }
    visited[curr] = true;
    const neighbors = map[curr];
    let res = isStart ? 0 : 1;
    for (let i = 0; i < neighbors.length; i++) {
        res += dfs(neighbors[i], visited, map, false);
    }
    return res;
}
```