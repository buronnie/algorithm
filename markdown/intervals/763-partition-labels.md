这道题本质上是一道interval的问题。我们只关心对于一个letter，它首次和末次出现的位置，然后依次merge interval，最后返回merged interval的长度。

```javascript
var partitionLabels = function(S) {
    const len = S.length;
    if (len === 0) {
        return [];
    }
    const map = {};
    for (let i = 0; i < len; i++) {
        if (!(S[i] in map)) {
            map[S[i]] = [i, i];
        } else {
            map[S[i]] = [map[S[i]][0], i];
        }
    }
    
    const intervals = [map[S[0]]];
    delete map[S[0]];
    for (let i = 1; i < len; i++) {
        const curr = map[S[i]];
        if (!curr) {
            continue;
        }
        const last = intervals[intervals.length-1];
        if (last[1] > curr[0]) {
            intervals.pop();
            intervals.push([last[0], Math.max(last[1], curr[1])]);
        } else {
            intervals.push([curr[0], curr[1]]);
        }
        delete map[S[i]];
    }
    return intervals.map(i => i[1] - i[0] + 1);
};
```