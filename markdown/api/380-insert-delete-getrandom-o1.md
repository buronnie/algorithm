这道题难点在于选择正确的数据结构。如果用array，getrandom和insert很容易O(1)， 但是删除不容易O(1)。考虑到这是一个set，数据的顺序无所谓，可以用一个map存储meta信息。
* arr存储数据
* map的key是数据值，value是对应的arr的下标

当删除一个数时，
* copy array中最后一个数到要删除数的位置, 更新map的值，删除要弹出的key, value，最后弹出array的最后一个数。有点狸猫换太子的意思，挺巧妙的。

```javascript
var RandomizedSet = function() {
    this.map = {};
    this.arr = [];
};

/**
 * Inserts a value to the set. Returns true if the set did not already contain the specified element. 
 * @param {number} val
 * @return {boolean}
 */
RandomizedSet.prototype.insert = function(val) {
    if (val in this.map) {
        return false;
    }
    this.arr.push(val);
    this.map[val] = this.arr.length-1;
    return true;
};

/**
 * Removes a value from the set. Returns true if the set contained the specified element. 
 * @param {number} val
 * @return {boolean}
 */
RandomizedSet.prototype.remove = function(val) {
    if (!(val in this.map)) {
        return false;
    }
    this.arr[this.map[val]] = this.arr[this.arr.length-1];
    this.map[this.arr[this.arr.length-1]] = this.map[val];
    this.arr.pop();
    delete this.map[val];
    return true;
};

/**
 * Get a random element from the set.
 * @return {number}
 */
RandomizedSet.prototype.getRandom = function() {
    return this.arr[Math.floor(Math.random() * this.arr.length)];
};

```