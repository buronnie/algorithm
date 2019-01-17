这个题需要一个一个把数据读出，所以不能一次性把array全部flat然后存起来. 既然是iterative method, 基本上是离不开stack的，需要存当前遍历的array及index。

输入：[[1, 1], 2, [3, 3]]
Stack: 

[[1, 1], 0]
[[[1,1], 2, [3,3]], 0]

如果当前是nestedList, 则把list取出入栈. 当pos到达array尾部，则出栈，并把栈头的pos + 1.

```javascript
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementationasdfa
 * function NestedInteger() {
 *
 *     Return true if this NestedInteger holds a single integer, rather than a nested list.
 *     @return {boolean}
 *     this.isInteger = function() {
 *         ...
 *     };
 *
 *     Return the single integer that this NestedInteger holds, if it holds a single integer
 *     Return null if this NestedInteger holds a nested list
 *     @return {integer}
 *     this.getInteger = function() {
 *         ...
 *     };
 *
 *     Return the nested list that this NestedInteger holds, if it holds a nested list
 *     Return null if this NestedInteger holds a single integer
 *     @return {NestedInteger[]}
 *     this.getList = function() {
 *         ...
 *     };
 * };
 */
/**
 * @constructor
 * @param {NestedInteger[]} nestedList
 */
var NestedIterator = function(nestedList) {
    this.stack = [[nestedList, 0]];
};


/**
 * @this NestedIterator
 * @returns {boolean}
 */
NestedIterator.prototype.hasNext = function() {
    while (this.stack.length > 0) {
        const [arr, pos] = this.stack[this.stack.length-1];
        if (arr.length === pos) {
            this.stack.pop();
            if (this.stack.length > 0) {
                this.stack[this.stack.length-1][1]++;
            }
        } else {
            const curr = arr[pos];
            if (curr.isInteger()) {
                return true;
            }
            this.stack.push([curr.getList(), 0]);
        }
    }
    return false;
};

/**
 * @this NestedIterator
 * @returns {integer}
 */
NestedIterator.prototype.next = function() {
    if (this.hasNext()) {
       const [arr, pos] = this.stack[this.stack.length-1];
        const res = arr[pos].getInteger();
        this.stack[this.stack.length-1][1]++
        return res; 
    }
    return null;
};

/**
 * Your NestedIterator will be called like this:
 * var i = new NestedIterator(nestedList), a = [];
 * while (i.hasNext()) a.push(i.next());
*/
```
