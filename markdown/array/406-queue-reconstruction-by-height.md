这道题挺有趣的，基本就是在考查面试者是否脑子够聪明。反正我捣鼓了半天也没想到最佳策略。下面的分析是根据discussion里的最佳答案来的：
* 排序时高个子前面有几个人是和比他低的人无关的，只和比他高或一样高的人有关。
* 前一句好像是废话，因为这就是(h, k)里关于k的定义。但这却是解题的关键。
* 当先排高个，因为高个排过后，低个不论放哪都不影响刚才高个的k
  
```javascript
var reconstructQueue = function(people) {
    const len = people.length;
    if (len <= 1) {
        return people;
    }
    people.sort((a, b) => {
        if (a[0] !== b[0]) {
            return b[0] - a[0];
        }
        return a[1] - b[1];
    });
    const res = [];
    for (let i = 0; i < len; i++) {
        res.splice(people[i][1], 0, [...people[i]]);
    }
        
    return res;
};
```