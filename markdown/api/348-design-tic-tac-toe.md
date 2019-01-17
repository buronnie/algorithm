这道题难点是如何快速的判断是否已经满足获胜条件。由于获胜条件是某一行/列/主副对角线全部是一种棋子，我们可以存储每一行/列/对角线，每一位选手的棋子总数。

```javascript
TicTacToe.prototype.move = function(row, col, player) {
    this.rowCount[player-1][row]++;
    this.colCount[player-1][col]++;
    if (row === col) {
        this.diagCount[player-1][0]++;
    }
    if (row === this.rows - col - 1) {
        this.diagCount[player-1][1]++;
    }
    if (this.rowCount[player-1][row] === this.rows ||
        this.colCount[player-1][col] === this.rows ||
        this.diagCount[player-1][0] === this.rows ||
        this.diagCount[player-1][1] === this.rows
       ) {
        return player;
    }
    return 0;
};adfa
```
