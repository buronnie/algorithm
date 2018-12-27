这道题非常巧妙的将tree的问题转化为graph的问题，而且这个解法是通用的，可以用在所有需要向上搜索parent的问题上。
* DFS建立无向图的map
* BFS搜索第K层的所有节点

```javascript
var distanceK = function(root, target, K) {
    if (root === null || K < 0) {
        return [];
    }
    if (K === 0) {
        return [target.val];
    }
    
    const map = new Map();
    dfs(root, map);

    const queue = map.get(target);
    if (!queue) {
        return [];
    }
    const visited = new Set();
    visited.add(target);
    let level = 1;
    while (queue.length > 0 && level < K) {
        const size = queue.length;
        for (let i = 0; i < size; i++) {
            const curr = queue.pop();
            const neighbors = map.get(curr);
            for (let j = 0; j < neighbors.length; j++) {
                if (!visited.has(neighbors[j])) {
                    queue.unshift(neighbors[j]);
                }
            }
            visited.add(curr);
        }
        level++;
    }
    return queue.map(a => a.val);
};

function dfs(root, map) {
    const left = root.left;
    const right = root.right;
    if (left !== null) {
        if (!map.get(root)) {
            map.set(root, [left]);
        } else {
            map.set(root, [...map.get(root), left]);
        }
        map.set(left, [root]);
        dfs(left, map);
    }
    if (right !== null) {
        if (!map.get(root)) {
            map.set(root, [right]);
        } else {
            map.set(root, [...map.get(root), right]);
        }
        map.set(right, [root]);
        dfs(right, map);
    }
}
```