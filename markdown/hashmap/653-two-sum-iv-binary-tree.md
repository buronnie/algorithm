这道题虽然输入是bst, 但是如果用hashmap来记录遇到点的值的话，可以推广到所有binary tree的情况。

```javascript
var findTarget = function(root, k) {
    if (root === null) {
        return false;
    }
    const set = new Set();
    return dfs(root, k, set);
};

function dfs(root, k, set) {
    if (root === null) {
        return false;
    }
    if (set.has(root.val)) {
        return true;
    }
    set.add(k-root.val);
    return dfs(root.left, k, set) || dfs(root.right, k, set);
}
```