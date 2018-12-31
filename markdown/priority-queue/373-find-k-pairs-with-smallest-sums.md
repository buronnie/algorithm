这类题的特点非常明显，只要是k smallest一般都可以用priority queue来解决，关键是如何确定下一步要入队的后选值。

这一题的难点是如何保证(i, j)只入队一次（不能重复），所以不可以无脑把(i+1, j) 和（i, j+1)都塞进去。
可以用hashSet来保证(i, j)只入队一次，更巧妙的方法是设计一种入队方法使每个值都能入队一次。

方法是：可以把(i, 0)先统一入队 i 从0到len1-1。之后每次只用把(i, j+1)入队即可。

```java
public List<int[]> kSmallestPairs(int[] nums1, int[] nums2, int k) {
    int len1 = nums1.length;
    int len2 = nums2.length;
    List<int[]> res = new ArrayList<>();
    if (k == 0 || len1 == 0 || len2 == 0) {
        return res;
    }
    Queue<int[]> queue = new PriorityQueue<>((a,b) -> nums1[a[0]] + nums2[a[1]] - nums1[b[0]] - nums2[b[1]]);
    for (int i = 0; i < len1; i++) {
        queue.offer(new int[]{i, 0});
    }
    while (queue.size() > 0 && k > 0) {
        int[] curr = queue.poll();
        int i = curr[0];
        int j = curr[1];
        res.add(new int[]{nums1[i], nums2[j]});
        if (j+1 < len2) {
            queue.offer(new int[]{i, j+1});
        }
        k--;
    }
    return res;
}
```