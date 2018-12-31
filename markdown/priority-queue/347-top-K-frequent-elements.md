这道题有两种解法：

* priority queue： 这是最无脑的解法，直接把(value, freq) 丢到priority queue里，然后弹出前k个

```java
public List<Integer> topKFrequent(int[] nums, int k) {
    int len = nums.length;
    List<Integer> res = new ArrayList<>();
    if (len == 0) {
        return res;
    }
    if (len == 1) {
        res.add(nums[0]);
        return res;
    }
    
    Map<Integer, Integer> map = new HashMap<>();
    Queue<Map.Entry<Integer, Integer>> queue = new PriorityQueue<>((a, b) -> b.getValue() - a.getValue());
    for (int num : nums) {
        if (!map.containsKey(num)) {
            map.put(num, 1);
        } else {
            map.put(num, map.get(num) + 1);
        }
    }
    for (Map.Entry<Integer, Integer> entry : map.entrySet()) {
        queue.offer(entry);
    }
    for (int i = 0; i < k; i++) {
        res.add(queue.poll().getKey());
    }
    return res;
}
```

* bucket sort: 这种方法仅局限于整数排序（range不太大）。思想是把要排序的数作为index（这里是freq)，然后按index从大到小扫描。

```javascript
var topKFrequent = function(nums, k) {
    const len = nums.length;
    if (len === 0 || k === 0) {
        return [];
    }
    const map = {};
    for (let num of nums) {
        if (num in map) {
            map[num]++;
        } else {
            map[num] = 1;
        }
    }
    
    const arr = new Array();
    let maxIndex = 0;
    for (let key in map) {
        const freq = map[key];
        if (!arr[freq]) {
            arr[freq] = [key];
        } else {
            arr[freq].push(key);
        }
        maxIndex = Math.max(maxIndex, freq);
    }
    
    const res = [];
    for (let i = maxIndex; i >= 0; i--) {
        if (arr[i]) {
            for (let j = 0; j < arr[i].length; j++) {
                res.push(arr[i][j]);
                k--;
                if (k === 0) {
                    return res;
                }
            }
        }
    }
    return res;
};
```