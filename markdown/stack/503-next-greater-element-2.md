这道题和496基本一样，只要先把array扩展为原来的一倍就可以处理circular的问题。

```javascript
var nextGreaterElements = function(nums) {
    const len = nums.length;
    if (len === 0) {
        return [];    
    }
    const res = new Array(len).fill(-1);
    nums = [...nums, ...nums.slice(0, len-1)];
    const stack = [];
    for (let i = 0; i < nums.length; i++) {
        if (stack.length === 0 || nums[stack[stack.length-1]] >= nums[i]) {
            stack.push(i);
        } else {
            while (stack.length > 0 && nums[stack[stack.length-1]] < nums[i]) {
                res[stack.pop()] = nums[i];
            }
            stack.push(i);
        }
    }
    
    return res.slice(0, len);
};
```