这个题搞了我好久才通过，思路比较直接，就是BFS找到最后一层，然后判断是否存在NULL介于NON-NULL之间。为了思路清晰，最好能定义单独的函数来作判断，比如判断是否是最后一层，判断最后一层是否有gap。这样书写可以保证面试时思路集中在某一个具体问题上，不容易出bug, 也会给面试官留下好印象，因为这样的代码更符合设计模式的要求。

```javascript
var isCompleteTree = function(root) {
    if (root === null) {
        return true;
    }
    const queue = [[root]];
    while (queue.length > 0) {
        const currLevel = queue.pop();
        const nextLevel = [];
        if (isBottom(currLevel)) {
            return !hasGap(currLevel);
        }
        if (currLevel.filter(a => a === null).length > 0) {
            return false;
        }
        for (let i = 0; i < currLevel.length; i++) {
            nextLevel.push(currLevel[i].left);
            nextLevel.push(currLevel[i].right);
        }
        queue.unshift(nextLevel);
    }
    return true;
};

function isBottom(arr) {
    for (let i = 0; i < arr.length; i++) {
        if (arr[i] !== null && (arr[i].left !== null || arr[i].right !== null)) {
            return false;
        }
    }
    return true;
}

function hasGap(arr) {
    const len = arr.length;
    let i = 0; 
    while (i < len && arr[i] !== null) {
        i++;
    }
    if (i === len) {
        return false;
    }
    let j = i;
    while (j < len && arr[j] === null) {
        j++;
    }
    
    if (j === len) {
        return false;
    }
    return true;
}
```