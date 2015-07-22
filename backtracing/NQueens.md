# N-Queens
## Source
leetcode:[51 N-Queens](https://leetcode.com/problems/n-queens/)

> The n-queens puzzle is the problem of placing n queens on an n×n chessboard
> such that no two queens attack each other.
> 
> ![one solution to the 8 queens puzzle](images/nqueens.png)
> 
> Given an integer n, return all distinct solutions to the n-queens puzzle.
> 
> Each solution contains a distinct board configuration of the n-queens' placement, where 'Q' and '.' both indicate a queen and an empty space respectively.
> 
> For example,
> 
> There exist two distinct solutions to the 4-queens puzzle:
> 
> <pre>
> [
>  [".Q..",  // Solution 1
>   "...Q",
>   "Q...",
>   "..Q."],
> 
>  ["..Q.",  // Solution 2
>   "Q...",
>   "...Q",
>   ".Q.."]
> ]
> </pre>


## 思路
采用回溯法遍历整个解空间。从第一行开始，依次尝试在每列放置皇后，一旦找到一个
可以放置皇后的列，便开始遍历下一行。否则，回退到上一行，尝试下一个列的位置。

一个位置能够放置皇后，需要满足以下条件：
* 该位置(i1,j1)不与任何已经放置皇后(i2,j2)的位置同列；
    
    即 j1!=j2
    
* 该位置(i1,j1)不与任何已经放置皇后(i2,j2)的位置同行；
    
    即 i1!=i2
    
* 该位置(i1,j1)不与任何已经放置皇后(i2,j2)的位置在同一对角线；
    
    即 |i1-i2| != |j1-j2|
    

## Java 代码
```        
public class Solution {
    private List<List<String>> res = new LinkedList<List<String>>();
    public List<List<String>> solveNQueens(int n) {
        char [][] board = new char[n][n];
        for (int i=0; i < n; i++) {
            Arrays.fill(board[i],'.');
        }
        placeQueen(board, 0, new LinkedList<Pair>());
        return res;
    }

    public void placeQueen(char[][] board, int row, LinkedList<Pair> queens) {
        if (row == board.length) {
            List<String> sol = new LinkedList<String>();
            for (int i=0; i < board.length; i++) {
                sol.add(new String(board[i]));
            }
            res.add(sol);
        }
        for (int j=0; j < board[0].length; j++) {
            if (isValid(new Pair(row,j), queens)) {
                board[row][j] = 'Q';
                queens.add(new Pair(row,j));
                placeQueen(board, row+1, queens);
                board[row][j] = '.';
                queens.pollLast();
            }
        }

    }

    public boolean isValid(Pair pos, LinkedList<Pair> queens) {
        for (Pair q: queens) {
            if (pos.x == q.x || pos.y == q.y)
                return false;

            if (Math.abs(pos.x - q.x) == Math.abs(pos.y - q.y))
                return false;
        }
        return true;
    }

    class Pair {
        int x;
        int y;
        public Pair(int x, int y) {
            this.x = x;
            this.y = y;
        }
    }
}
```
