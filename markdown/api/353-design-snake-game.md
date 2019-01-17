这道题非常有意思，难度不大但是想bug free也不容易。如果思路清晰则代码会非常简洁。
* 移动头部到下一个cell
* 判断此时头部是否已经越界
* 判断是否当前cell有食物，如果没有则弹出蛇尾。判断当前cell是否和蛇collide，如果没有则压入cell作为新的蛇头
adfas
```javascript
/**
 * Initialize your data structure here.
        @param width - screen width
        @param height - screen height 
        @param food - A list of food positions
        E.g food = [[1,1], [1,0]] means the first food is positioned at [1,1], the second is at [1,0].
 * @param {number} width
 * @param {number} height
 * @param {number[][]} food
 */
var SnakeGame = function(width, height, food) {
    this.rows = height;
    this.cols = width;
    this.food = food;
    this.foodIdx = 0;
    this.x = 0;
    this.y = 0;
    this.snake = [[0, 0]];
};

/**
 * Moves the snake.
        @param direction - 'U' = Up, 'L' = Left, 'R' = Right, 'D' = Down 
        @return The game's score after the move. Return -1 if game over. 
        Game over when snake crosses the screen boundary or bites its body. 
 * @param {string} direction
 * @return {number}
 */
SnakeGame.prototype.move = function(direction) {
    if (direction === 'U') {
        this.x--;
    } else if (direction === 'D') {
        this.x++;
    } else if (direction === 'L') {
        this.y--;
    } else {
        this.y++;
    }
    
    if (!this.isValid(this.x, this.y)) {
        return -1;
    }
    
    if (!this.process(this.x, this.y)) {
        return -1;
    }
    return this.snake.length - 1;
};

SnakeGame.prototype.isValid = function (x, y) {
    return x >= 0 && x < this.rows && y >= 0 && y < this.cols;
}

SnakeGame.prototype.process = function (x, y) {
    if (this.foodIdx === this.food.length) {
        this.snake.pop();
    } else if (this.food[this.foodIdx][0] === x && this.food[this.foodIdx][1] === y) {
        this.foodIdx++;
    } else {
        this.snake.pop();
    }
    for (let pos of this.snake) {
        if (pos[0] === x && pos[1] === y) {
            return false;
        }
    }
    this.snake.unshift([x, y]);
    return true;
}

/** 
 * Your SnakeGame object will be instantiated and called as such:
 * var obj = Object.create(SnakeGame).createNew(width, height, food)
 * var param_1 = obj.move(direction)
 */
```
