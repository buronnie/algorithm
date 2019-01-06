这道题可以按如下的步骤操作：
1. input -> digit array (arr)
2. clone array and sort (sorted)
3. scan from i = 0, and find the first digit where arr[i] != sorted[i]
4. scan from i = len-1, and find the first max digit (best deal to swap the lowest position)
5. swap

```javascript
var maximumSwap = function(num) {
    const arr = String(num).split('');
    const sorted = [...arr];
    sorted.sort((a, b) => b - a);
    const len = arr.length;
    
    let i = 0;
    while (i < len && arr[i] === sorted[i]) i++;
    if (i === len) {
        return num;
    }
    const left = i;
    i = len-1;
    while (i > left && arr[i] !== sorted[left]) i--;
    swap(arr, left, i);
    return parseInt(arr.join(''));
};

function swap(arr, i, j) {
    const tmp = arr[i];
    arr[i] = arr[j];
    arr[j] = tmp;
}
```