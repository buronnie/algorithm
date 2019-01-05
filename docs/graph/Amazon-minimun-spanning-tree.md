问题：最小的Cost连接所有的仓库
input: [[1, 0, 1],[7,0,2],[5,1,2],[4,1,3],[3,1,4],[6,2,4],[2,3,4]]
output: 11

经典问题，直接记住算法即可。
* 按weight对edges进行排序
* 依次添加edge, 保证新的edge不会造成cycle, 更新edge两端节点的parent

```javascript
function root(x, parent) {
    while (x !== parent[x]) {
        x = parent[x];
    }
    return x;
}
function union(x, y, parent) {
    const p = root(x, parent);
    const q = root(y, parent);
    parent[p] = parent[q];
}
// connection: [weight, node1, node2]
// n: total number of nodes
function mst(connections, n) {
    const len = connections.length;
    if (len === 0) {
        return 0;
    }
    const parent = {};
    for (let i = 0; i < n; i++) {
        parent[i] = i;
    }
    
    connections.sort((a, b) => a[0] - b[0]);
    let minCost = 0;
    const connectedNodeSet = new Set();
    for (let connection of connections) {
        const [weight, x, y] = connection;
        if (root(x, parent) !== root(y, parent)) {
            minCost += weight;
            union(x, y, parent);
            connectedNodeSet.add(x);
            connectedNodeSet.add(y);
        }
    }
    if (connectedNodeSet.size < n) {
        return -1;
    }
    return minCost;
}
```