这道题可以从人的思维方式入手。如果是人来解决这个问题，会先找到最内层的[]，然后decode之间的string。重复这个过程直到所有的[]都被decode。

```javascript
var decodeString = function(s) {
    let len = s.length;
    if (len === 0) {
        return '';
    }
    while (true) {
        // find first ']' and corresponding matched '['
        let i = 0; 
        len = s.length;
        while (i < len && s[i] !== ']') i++;
        if (i === len) {
            return s;
        }
        let j = i-1;
        while (j >= 0 && s[j] !== '[') j--;
        
        const word = s.substring(j+1, i);
        let k = j-1;
        while (k >= 0 && !isNaN(s[k])) k--;
        const number = parseInt(s.substring(k+1, j));
        let str = '';
        for (let ii = 0; ii < number; ii++) {
            str += word;
        }
        const arr = s.split('');
        arr.splice(k+1, i-k, str);
        s = arr.join('');
        if (!encoded(s)) {
            break;
        }
    }
    return s;
    
};

function encoded(s) {
    for (let i = 0; i < s.length; i++) {
        if (s[i] === '[' || s[i] === ']') {
            return true;
        }
    }
    return false;
}
```

论坛里有人用dfs来解决这个问题，关键是把pos以引用方式传入，这样scan一遍就可以decode整个string。

```javascript
var decodeString = function(s) {
    let len = s.length;
    if (len === 0) {
        return '';
    }
    
    const pos = [0];
    return helper(s, pos);    
};

function helper(str, pos) {
    const len = str.length;
    if (pos === len) {
        return '';
    }
    
    let num = 0;
    let res = '';
    for (;pos[0] < len; pos[0]++) {
        const curr = str[pos[0]];
        if (!isNaN(curr)) {
            num = num*10 + parseInt(str[pos[0]]);
        } else if (curr === '[') {
            pos[0]++;
            word = helper(str, pos);
            for (let i = 0; i < num; i++) {
                res += word;
            }
            num = 0;
        } else if (curr === ']') {
            return res;
        } else {
            res += curr;
        }
    }
    return res;
}
```