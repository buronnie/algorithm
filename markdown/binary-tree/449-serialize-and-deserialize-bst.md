这道题和297非常像，不同的是这里不需要preorder 和inorder去重建binary tree, 因为bst的特点已经足够我们找到左右子树。

```javascript
var serialize = function(root) {
    if (root === null) {
        return '';
    }
    const res = dfs(root);
    return res;
};

function dfs(root) {
    if (root === null) {
        return '';
    }
    if (root.left === null && root.right === null) {
        return String(root.val);
    }
    if (root.left === null) {
        return `${root.val},${dfs(root.right)}`;
    }
    if (root.right === null) {
        return `${root.val},${dfs(root.left)}`;
    }
    const left = dfs(root.left);
    const right = dfs(root.right);
    return `${root.val},${left},${right}`;
}

/**
 * Decodes your encoded data to tree.
 *
 * @param {string} data
 * @return {TreeNode}
 */
var deserialize = function(data) {
    if (data === '') {
        return null;
    }
    const arr = data.split(',');
    return helper(arr);
};

function helper(arr) {
    if (arr.length === 0) {
        return null;
    }
    const curr = parseInt(arr[0]);
    let i = 1; 
    while (i < arr.length && arr[i] !== '' && parseInt(arr[i]) < curr) {
        i++;
    }
    const root = new TreeNode(curr);
    root.left = helper(arr.slice(1, i));
    root.right = helper(arr.slice(i));
    return root;
}
```