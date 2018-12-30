这道题写法不难，但是思路相当经典。需要考虑经过root节点和不经过root节点两种情况。

```javascript
var pathSum = function(root, sum) {
    if (root === null) {
        return 0;
    }
    return pathSumFrom(root, sum) + pathSum(root.left, sum) + pathSum(root.right, sum);
};

function pathSumFrom(root, sum) {
    if (root === null) {
        return 0;
    }
    return (root.val === sum ? 1 : 0) + 
        pathSumFrom(root.left, sum - root.val) +
        pathSumFrom(root.right, sum - root.val);
}
```