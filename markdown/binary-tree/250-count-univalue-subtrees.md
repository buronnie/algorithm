这道题是一道经典的树的分治问题。在每一个节点上需要知道两个东西：1. 当前子树是否为unival tree; 2. 当前子树总共有多少个unival tree. 

```javascript
var countUnivalSubtrees = function(root) {
    const [isUnivalTree, total] = helper(root);
    return total;
};

function helper(root) {
    if (root === null) {
        return [true, 0];
    }
    
    const [leftIsUnivalTree, leftTotal] = helper(root.left);
    const [rightIsUnivalTree, rightTotal] = helper(root.right);
    const isUnivalTree = checkUnivalTree(root, leftIsUnivalTree, rightIsUnivalTree);
    const total = leftTotal + rightTotal + (isUnivalTree ? 1 : 0);
    return [isUnivalTree, total];
}
           
function checkUnivalTree(root, leftIsUnivalTree, rightIsUnivalTree) {
    if (!leftIsUnivalTree || !rightIsUnivalTree) {
        return false;
    }
    if (root.left === null && root.right === null) {
        return true;
    }
    if (root.left === null) {
        return root.val === root.right.val;
    }
    if (root.right === null) {
        return root.val === root.left.val;
    }
    return root.val === root.left.val && root.val === root.right.val;
}
```