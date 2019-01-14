这道题思路很简单，就是dfs, 关键是得知道几个关于file system的api

```java
fs.readdirSync(path) - 读目录中的文件列表
fs.lstatSync(path) - 读文件的metadata
fs.lstatSync(path).isDirectory() - 判断是否是目录
fs.lstatSync(path).isFile() - 判断是否是目录
path.basename(path) - 得到path中的文件名
```

```javascript
const fs = require('fs');
const pathUtil = require('path');

function find(path, nameRegex, sizeCondition) {
    const res = [];
    dfs(path, nameRegex, sizeCondition, res);
    return res;
}

function dfs(path, nameRegex, sizeCondition, res) {
    const stat = fs.lstatSync(path);
    if (stat.isDirectory()) {
        fs.readdirSync(path).forEach((file) => {
            dfs(path + '/' + file, nameRegex, sizeCondition, res);
        });
    } else {
        let sizeConditionSatisfied;
        if (sizeCondition) {
            if (sizeCondition.oper === '>' && stat.size >= sizeCondition.value) {
                sizeConditionSatisfied = true;
            } else if (sizeCondition.oper === '=' && stat.size === sizeCondition.value) {
                sizeConditionSatisfied = true;
            } else if (sizeCondition.oper === '<' && stat.size <= sizeCondition.value) {
                sizeConditionSatisfied = true;
            } else {
                sizeConditionSatisfied = false;
            }
        } else {
            sizeConditionSatisfied = true;
        }
        if (sizeConditionSatisfied && nameRegex.test(pathUtil.basename(path))) {
            res.push(path);
        }
    }
}
```