这道题是一道经典的带权重的有向图问题。既可以用BFS，也可以用DFS来解决。

* BFS: 难点在于每个加入队列的节点需要包含到目前为止的accumulated cost。一开始超时，需要在入队前判断accumulated cost是否已经比当前的min cost大，如果大就没有必要再入队了。

```javascript
var findCheapestPrice = function(n, flights, src, dst, K) {
    if (n === 0) {
        return 0;
    }
    const map = {};
    for (let i = 0; i < n; i++) {
        map[i] = [];
    }
    const cost = {};
    for (let i = 0; i < flights.length; i++) {
        map[flights[i][0]].push(flights[i][1]);
        cost[flights[i][0]+','+flights[i][1]] = flights[i][2];
    }
    
    const queue = [[src, 0]];
    let level = 0;
    let min = Number.MAX_SAFE_INTEGER;
    while (queue.length > 0 && level <= K+1) {
        const size = queue.length;
        for (let i = 0; i < size; i++) {
            const [curr, accumCost] = queue.pop();
            if (curr === dst) {
                min = Math.min(min, accumCost);
            } else {
                for (let j = 0; j < map[curr].length; j++) {
                    if (accumCost + cost[curr+','+map[curr][j]] >= min) continue;
                    queue.unshift([map[curr][j], accumCost + cost[curr+','+map[curr][j]]]);
                }
            }
        }
        level++;
    }
    return min === Number.MAX_SAFE_INTEGER ? -1 : min;
};
```

* DFS: 同样要prone优化，否则会超时。
  
```javascript
var findCheapestPrice = function(n, flights, src, dst, K) {
    if (n === 0) {
        return 0;
    }
    if (src === dst) {
        return 0;
    }
    const map = {};
    for (let i = 0; i < n; i++) {
        map[i] = [];
    }
    const cost = {};
    for (let i = 0; i < flights.length; i++) {
        map[flights[i][0]].push(flights[i][1]);
        cost[flights[i][0]+','+flights[i][1]] = flights[i][2];
    }
    
    const min = [Number.MAX_SAFE_INTEGER];
    dfs(map, cost, src, dst, K, 0, min);
    return min[0] === Number.MAX_SAFE_INTEGER ? -1 : min[0];
};

function dfs(map, cost, src, dst, K, sum, min) {
    if (src === dst) {
        min[0] = Math.min(min[0], sum);
        return;
    }
    if (K < 0) {
        return;
    }
    const neighbors = map[src];
    for (let i = 0; i < neighbors.length; i++) {
        const next = neighbors[i];
        if (sum + cost[src+','+next] >= min[0]) {
            continue;
        }
        dfs(map, cost, next, dst, K-1, sum + cost[src+','+next], min);
    }
}
```