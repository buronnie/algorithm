这道题乍一看没有头绪，其实不难。找duplicate肯定是往hashmap上想，检查子树结构是否相同应该考虑如何对子树编码，利用分治法在每个节点处得到该子树的编码，然后看是否在hashmap中。

```javascript
var findDuplicateSubtrees = function(root) {
    if (root === null) {
        return [];
    }
    const map = {};
    helper(root, map);
    let res = [];
    for (let key in map) {
        if (map[key].length > 1) {
            res.push(map[key][0]);
        }
    }
    return res;
};

function helper(root, map) {
    if (root === null) {
        return 'null';
    }
    const left = helper(root.left, map);
    const right = helper(root.right, map);
    const code = `${root.val}->${left}->${right}`;
    if (code in map) {
        map[code].push(root);
    } else {
        map[code] = [root];
    }
    return code;
}
```