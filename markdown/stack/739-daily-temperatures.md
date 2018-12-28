这道题本质上是维护一个min stack，当遇到破坏min stack的数时就先pop, 再push。技巧是把index入栈而不是value本身。

```javascript
var dailyTemperatures = function(T) {
    const len = T.length;
    if (len === 0) {
        return [];
    } 
    if (len === 1) {
        return [0];
    }
    
    const stack = [];
    const res = new Array(len);
    for (let i = 0; i < len; i++) {
        res[i] = 0;
    }
    for (let i = 0; i < len; i++) {
        if (stack.length === 0 || T[i] <= T[stack[stack.length-1]]) {
            stack.push(i);
            continue;
        }
        while (stack.length > 0) {
            if (T[i] > T[stack[stack.length-1]]) {
                const prevIdx = stack.pop();
                res[prevIdx] = i - prevIdx;
            } else {
                break;
            }
        }
        stack.push(i);
    }
    return res;
};
```