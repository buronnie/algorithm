这道题当看到example2的N=100000000时，就应该意识到状态肯定会循环重复。只要记录每次的状态，直到检测到状态重复时，就可以算出循环起始位置和循环长度。
```
pos = (N - periodStart) % period + periodStart
```

```javascript
var prisonAfterNDays = function(cells, N) {
    const len = cells.length;
    if (len === 0 || N === 0) {
        return [];
    }
    
    const map = {[cells.join('')]: 0};
    const states=  [cells];
    let i = 1;
    let period = 0;
    let periodStartPos = 0;
    while (true) {
        const nextState = getNextCell(cells);
        const nextStateStr = nextState.join('');
        if (nextStateStr in map) {
            periodStartPos = map[nextStateStr];
            period = states.length - map[nextStateStr];
            break;
        } else {
            map[nextStateStr] = i;
        }
        states.push(nextState);
        cells = nextState;
        i++;
    }
    return states[(N-periodStartPos) % period + periodStartPos];
};

function getNextCell(cells) {
    const len = cells.length;
    const next = new Array(len);
    next[0] = 0; 
    next[len-1] = 0;
    for (let i = 1; i< len-1; i++) {
        if (cells[i-1] === cells[i+1]) {
            next[i] = 1;
        } else {
            next[i] = 0;
        }
    }
    return next;
}
```