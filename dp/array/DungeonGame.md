# Dungeon Game 
## Source
leetcode:[Dungeon Game](https://leetcode.com/problems/dungeon-game/)
> The demons had captured the princess (P) and imprisoned her in the bottom-right
> corner of a dungeon. The dungeon consists of M x N rooms laid out in a 2D grid.
> Our valiant knight (K) was initially positioned in the top-left room and must
> fight his way through the dungeon to rescue the princess.
> 
> The knight has an initial health point represented by a positive integer. If at
> any point his health point drops to 0 or below, he dies immediately.
> 
> Some of the rooms are guarded by demons, so the knight loses health (negative
> integers) upon entering these rooms; other rooms are either empty (0's) or
> contain magic orbs that increase the knight's health (positive integers).
> 
> In order to reach the princess as quickly as possible, the knight decides to
> move only rightward or downward in each step.
> 
> 
> Write a function to determine the knight's minimum initial health so that he is
> able to rescue the princess.
> 
> For example, given the dungeon below, the initial health of the knight must be
> at least 7 if he follows the optimal path RIGHT-> RIGHT -> DOWN -> DOWN.
> 
> | -2 (K) | -3  | 3      |
> | --     | --  | --     |
> | -5     | -10 | 1      |
> | 10     | 30  | -5 (P) |
> 
> ### Notes:
> 
> * The knight's health has no upper bound.
> * Any room can contain threats or power-ups, even the first room the knight enters
> * and the bottom-right room where the princess is imprisoned.

## 思路
注意题目中的一个要求：the knight decides to move only rightward or downward in each step,
骑士每次只能向右或者向下走,避免了回头路。否则的话，到达每个点都要遍历四个方向。

下面来列动态规划方程：

    dp[i][j] = Max((Min(dp[i+1][j], dp[i][j+1]) - dungeon[i][j]), 1)
    
其中，
* `dp[i][j]` 表示如果希望或者到达第(M,N)格，骑士在到达第(i,j)格时必须拥有的最低血量
* `dungeon[i][j]` 表示骑士到达第(i,j)格是，会受到的伤害值(负值)/回血值(正值)

下面来分步解释这个动规方程的含义：
* `Min(dp[i+1][j], dp[i][j+1])`表示骑士走到下一步需要的最低血量
* `Min(dp[i+1][j], dp[i][j+1]) - dungeon[i][j]` 表示骑士为了能走到下一步，
    当前格需要拥有的最低血量, 因为本格会有消耗嘛。
* `Max((Min(dp[i+1][j], dp[i][j+1]) - dungeon[i][j]), 1)`
    表示如果当前格需要的最低血量是负的，那么让它为1，保证骑士不死即可。
    
剩下的就是边界条件了：边界条件是最后一行和最后一列：
* 最后一行和最后一列的交点(第(M,N)格),也是起点，它只需要考虑自己即可：
 
    `dp[M-1][N-1] = dungeon[M-1][N-1]>=0 ? 1 : -dungeon[M-1][N-1] + 1`
    
    或是
    
    `dp[M-1][N-1] = Max(0-dungeon[M-1][N-1], 1)`
    
    如果该格不会造成伤害，那么骑士只需要拥有血量1即可，否则，骑士拥有的血量要比伤害值多1.
* 最后一行的其它元素, 只需要考虑右边的元素:

    `dp[M-1][j] = Max((dp[M-1][j+1] - dungeon[M-1][j]), 1)`

* 最后一列的其它元素，只需要考虑下边的元素：

    `dp[i][N-1] = Max((dp[i+1][N-1] - dungeon[i][N-1]), 1)`
    

从上，我们可以看到，边界条件只是动规方程的一种特例而已。


为什么我们要从终点开始遍历，而不是起点呢？原因是起点的值我们并不知道。

## Java DP 解法
``` java
public class Solution {
    public int calculateMinimumHP(int[][] dungeon) {
        if (dungeon == null || dungeon.length == 0 ||
                dungeon[0] == null || dungeon[0].length == 0)
            return 1;

        int xlen = dungeon.length;
        int ylen = dungeon[0].length;

        int [][] dp = new int[xlen][ylen];

        dp[xlen-1][ylen-1] = Math.max(0-dungeon[xlen-1][ylen-1]+1,1);
        for (int i=xlen-2; i >= 0; i--) {
            dp[i][ylen-1] = Math.max(dp[i+1][ylen-1]-dungeon[i][ylen-1], 1);
        }

        for (int j=ylen-2; j >= 0; j--) {
            dp[xlen-1][j] = Math.max(dp[xlen-1][j+1]-dungeon[xlen-1][j], 1);
        }

        for (int i=xlen-2; i >= 0; i--) {
            for (int j=ylen-2; j >= 0; j--) {
                dp[i][j] = Math.max( Math.min(dp[i][j+1],dp[i+1][j])-dungeon[i][j], 1);
            }
        }

        return dp[0][0];
    }
}
```
