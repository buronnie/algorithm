这道题虽然是hard, 但是思路比较直接。首先要看到这是一个graph的问题，连通的节点不是bus stop，而是bus/route。
* 对每一个bus route建立stop map或set，方便以后查询
* 利用前一步的stop map, 两两比较bus route，确定是否两个route之间有相同的stop,如果有则这两个routes是连通的
* 对起点和终点找到所有bus routes包含起点或终点
* 以所有包括起点的bus routes开始进行bfs，当某个route在终点所包括的routes中，返回当前的level

```javascript
var numBusesToDestination = function(routes, S, T) {
    const numBuses = routes.length;
    if (numBuses === 0) {
        return -1;
    }
    if (S === T) {
        return 0;
    }
    const stops = [];
    for (let i = 0; i < numBuses; i++) {
        const map = {};
        const route = routes[i];
        for (let j = 0; j < route.length; j++) {
            map[route[j]] = true;
        }
        stops.push(map);
    }
    
    const connection = {};
    for (let i = 0; i < numBuses; i++) {
        connection[i] = [];
    }
    for (let i = 0; i < numBuses; i++) {
        for (let j = i+1; j < numBuses; j++) {
            if (hasConnection(stops[i], stops[j])) {
                connection[i].push(j);
                connection[j].push(i);
            }            
        }
    }
    
    // find which routes include start and end
    const routesIncludeStartList = [...findStopFromRoutes(stops, S)];
    const routesIncludeEndSet = findStopFromRoutes(stops, T);
    
    const queue = routesIncludeStartList;
    let level = 1;
    const visited = new Set();
    while (queue.length > 0) {
        const size = queue.length;
        for (let i = 0; i < size; i++) {
            const curr = queue.pop();
            if (routesIncludeEndSet.has(curr)) {
                return level;
            }
            for (let j = 0; j < connection[curr].length; j++) {
                if (!visited.has(connection[curr][j])) {
                    visited.add(connection[curr][j]);
                    queue.unshift(connection[curr][j]);
                }
            }
        }
        level++;
    }
    return -1;
};

function findStopFromRoutes(stops, S) {
    const res = new Set();
    for (let i = 0; i < stops.length; i++) {
        const routeMap = stops[i];
        if (S in routeMap) {
            res.add(i);
        }
    }
    return res;
}

function hasConnection(map1, map2) {
    for (let key in map1) {
        if (key in map2) {
            return true;
        }
    }
    return false;
}
```