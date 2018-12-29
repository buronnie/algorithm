一看到 next greater number 就应该想到stack, 这道题和739非常类似。都是要维护一个min stack，当遇到破坏min stack的数时就先pop, 再push。技巧是把index入栈而不是value本身。

```javascript
var nextGreaterElement = function(nums1, nums2) {
    const len1 = nums1.length;
    const len2 = nums2.length;
    if (len1 === 0) {
        return [];
    }
    if (len2 === 0) {
        return new Array(len1).fill(-1);
    }
    
    const map = {};
    const res = new Array(len1).fill(-1);
    for (let i = 0; i < len1; i++) {
        map[nums1[i]] = i;
    }
    
    const stack = [];
    let i = 0;
    while (i < len2) {
        if (stack.length === 0 || nums2[stack[stack.length-1]] >= nums2[i]) {
            stack.push(i);
            i++;
            continue;
        }
        while (stack.length > 0 && nums2[stack[stack.length-1]] < nums2[i]) {
            const valueInNums2 = nums2[stack.pop()];
            if (valueInNums2 in map) {
                res[map[valueInNums2]] = nums2[i];
            }
        }
        stack.push(i);
        i++;
    }
    return res;
    
};
```