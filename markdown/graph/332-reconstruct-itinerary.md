有向图问题，一开始考虑用bfs, 但是因为path有可能会走到dead end，所以bfs并不适合。dfs的探路方式是一条道走到底，如果发现是dead end，则试下一条路。
这道题还要求我们输入lexical order尽可能小的路径。所以我们对neighbor节点要进行排序。

* 构造adjacent map
* 对每个value array进行排序
* dfs遍历，注意对value array 里的每一个值, 要先弹出，再恢复

```javascript
var findItinerary = function(tickets) {
    const len = tickets.length;
    if (len === 0) {
        return [];
    }
    
    const map = {};
    for (let ticket of tickets) {
        if (ticket[0] in map) {
            map[ticket[0]].push(ticket[1]);
        } else {
            map[ticket[0]] = [ticket[1]];
        }
    }
    for (let key in map) {
        map[key].sort();
    }
    const curr = ['JFK'];
    const res = [];
    dfs(map, tickets.length + 1, curr, res);
    return res[0];
};

function dfs(map, city, routeLength, curr, res) {
    const city = curr[curr.length - 1];
    if (curr.length === routeLength) {
        res.push([...curr]);
        return true;
    }
    
    const neighbors = map[city];
    if (!neighbors) {
        return false;
    }
    for (let i = 0; i < neighbors.length; i++) {
        const neighbor = neighbors[i];
        neighbors.splice(i, 1);
        curr.push(neighbor);
        if (dfs(map, routeLength, curr, res)) {
            return true;
        }
        curr.pop();
        neighbors.splice(i, 0, neighbor);
    }
}
```