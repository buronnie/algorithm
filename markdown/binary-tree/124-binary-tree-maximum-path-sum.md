这是一道伪hard题。分治的思想，每一次返回任何path的最大和（可以不经过root)，还有经过root的单边path最大和。

```javascript
var maxPathSum = function(root) {
    if (root === null) {
        return 0;
    }
    
    const [max, _] = dfs(root);
    return max;
};

function dfs(root) {
    if (root === null) {
        return [-Number.MAX_SAFE_INTEGER, -Number.MAX_SAFE_INTEGER];
    }
    
    const [twoWayMax1, oneWayMax1] = dfs(root.left);
    const [twoWayMax2, oneWayMax2] = dfs(root.right);
    
    const oneWayMaxCurr = root.val + Math.max(Math.max(oneWayMax1, 0), Math.max(oneWayMax2, 0));
    const twoWayMaxCurr = Math.max(twoWayMax1, twoWayMax2, root.val + Math.max(oneWayMax1, 0) + Math.max(oneWayMax2, 0));
    
    return [twoWayMaxCurr, oneWayMaxCurr];
}
```