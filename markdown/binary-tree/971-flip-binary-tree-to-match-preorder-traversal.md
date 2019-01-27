这道题一开始把题意理解错了，以为是只交换node的左右子节点的值，其实是把整个左右子树完全翻转。用dfs去想会比较直观，当我们发现左边子树和voyage的值不匹配时，就要交换左右子树。当前节点必须要匹配，否则无解。比较tricky的是当一个node只有右子树时，是不需要交换的，所以最好的比较方法是比较右边子树的节点是否和下一个voyage的值匹配，而不是用左子树和voyage比较。

iteration的解法：
```javascript
var flipMatchVoyage = function(root, voyage) {
    if (root === null) {
        return [];
    }
    const res = [];
    const stack = [root];
    let i = 0;
    while (stack.length > 0) {
        const curr = stack.pop();
        if (curr === null) {
            continue;
        }
        if (curr.val !== voyage[i]) {
            return [-1];
        }
        i++;
        if (curr.right && curr.right.val === voyage[i]) {
            if (curr.left) {
                res.push(curr.val);
            }
            stack.push(curr.left);
            stack.push(curr.right);
        } else {
            stack.push(curr.right);
            stack.push(curr.left);
        }
    }
    return res;
};
```

dfs解法：
```javascript
var flipMatchVoyage = function(root, voyage) {
    const res = [];
    const val = dfs(root, voyage, res, [0]);
    return !val ? [-1] : res;
};

function dfs(root, voyage, res, pos) {
    if (root === null) {
        return true;
    }
    if (root.val !== voyage[pos[0]]) {
        return false;
    }
    pos[0]++;
    if (root.right && root.right.val === voyage[pos[0]]) {
        if (root.left) {
            res.push(root.val);
        }
        return dfs(root.right, voyage, res, pos) && dfs(root.left, voyage, res, pos); 
    }
    return dfs(root.left, voyage, res, pos) && dfs(root.right, voyage, res, pos); 
}
```