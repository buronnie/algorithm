问题：设计chess game?

Classes:
* Game
  - Board
  - two players

* Board
  - 2-D 8x8 cells
  - init(): initialize array and pieces
  - canMove(command)
  - movePiece()
  - removePiece()

* Cell
  - piece
  - x, y coordinates

* Piece (abstract class)
  - x, y coordiates
  - isValidMove(board, fromX, fromY, toX, toY)
  - live: is alive or dead

* Player
  - list of pieces
  - list of commands
  - meta data (username, score)

* Command
  - Piece
  - Destination

```java
public class Game {
    private final static Board board;
    private Player p1;
    private Player p2;
    public Game(username1, username2) {
        board = new Board();
        p1 = new Player(username1);
        p2 = new Player(username2);
    }

    public startGame() {
        while (true) {
            process(p1);
            if (board.getWin()) {
                System.out.println("P1 win");
                break;
            }
            process(p2);
            if (board.getWin()) {
                System.out.println("P2 win");
                break;
            }
        }
    }

    private process(Player p) {
        Command cmd = new Command(input);
        p.addCommand(cmd);
        board.executeCommand(p);
    }
}

public class Board {
    private Spot[][] spots;
    private boolean win;

    public Board() {
        win = false;
        spots = new Spot[8][8];
    }

    public void init(Player p) {

    }

    public executeCommand(Player p) {
        Command cmd = p.getCurrentCommand(p);
        if (!piece.validMove(this, cmd.curX, cmd.curY, cmd.desX, dstY)) {
            p.removeCurrentCmd();
            return false;
        }
        if (spot[cmd.desX][cmd.desY] != null && spot[cmd.desX][cmd.desY].color == piece.color) {
            return false;
        }
        spot[cmd.desX][cmd.desY].occupy(piece);
        if (piece.getClass().getName().equals("King")) {
            win = true;
        }
        return true;
    }
}
```