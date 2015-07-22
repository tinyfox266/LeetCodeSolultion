# N-Queens II 
## Source
leetcode:[51 N-Queens](https://leetcode.com/problems/n-queens-ii/)
> Follow up for N-Queens problem.
> 
> Now, instead outputting board configurations, return the total number of
> distinct solutions.
> 
> ![one solution to 8 queens puzzle](images/nqueens.png)

## 思路
这道题相比于[N-Queens](NQueens.md)问题,只要求返回解的个数即可。所以只要在
[N-Queens](NQueens.md)的解法中，将保存找到的解改为解的个数+1即可。

## Java 代码
``` java
public class Solution {
     int res=0;
    public int totalNQueens(int n) {
        placeQueen(n, 0, new LinkedList<Pair>());
        return res;
    }

    public void placeQueen(int size, int row, LinkedList<Pair> queens) {
        if (row == size) {
            res++;
        }
        for (int j=0; j < size; j++) {
            if (isValid(new Pair(row,j), queens)) {
                queens.add(new Pair(row,j));
                placeQueen(size, row+1, queens);
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
