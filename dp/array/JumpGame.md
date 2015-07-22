# Jump Game
## Source
leetcode:[55 Jump Game](https://leetcode.com/problems/jump-game/).
> Given an array of non-negative integers, you are initially positioned at the
> first index of the array.
> 
> Each element in the array represents your maximum jump length at that position.
> 
> Determine if you are able to reach the last index.
> 
> For example:
> 
> A = [2,3,1,1,4], return true.
> 
> A = [3,2,1,0,4], return false.

## 思路
如果从位置i开始可以跳到最后的位置，那么需要有一个从位置i可达的位置能够达到最后的位置。

动规方程为:

`dp[i] = true if exists i < j <= i+nums[i] such that dp[j]=true`

`dp[i] = false otherwise`

其中，
* dp[i]表示从位置i是否能达到最后的位置，`true`表示可达，`false`表示不可达。
* nums[i]表示从第i个位置开始，能够向后跳的最大步数。

具体代码见Java DP代码一。

这个代码没有通过leetcode OJ的大数据（超时）。

下面，我们来看一下，怎样能够剪枝，减少不必要的运算。

我们可以通过下属性质来剪枝：
* 如果从第i+1个位置不能到达最后的位置，而且第i+1个位置能够跳的步数比第i个位置
    能够跳的步数还多，那第i个位置也到达不了最后位置。
* 如果从第i+1个位置能到达最后的位置，而且第i+1个位置能够跳的步数比第i个位置
    能够跳的步数还少，那第i个位置也能到达最后位置。
    
于是有了Java DP代码二。


## Java DP 代码一
``` java
public class Solution {
    public boolean canJump(int[] nums) {
        if (nums == null || nums.length == 0)
            return false;

        if (nums.length == 1)
            return true;

        boolean [] canJumpArray = new boolean[nums.length];
        canJumpArray[nums.length-1] = true;
        for (int i=nums.length-2; i >= 0; i--) {
            int count=nums[i];
            boolean succ = false;
            for (int j=count; j >= 1; j--) {
                if (i+j < nums.length && canJumpArray[i+j]) {
                    succ = true;
                    break;
                }

            }
            canJumpArray[i] = succ;
        }

        return canJumpArray[0];
    }
}
```

## Java DP 代码二
``` java
public class Solution {
    public boolean canJump(int[] nums) {
        if (nums == null || nums.length == 0)
            return false;

        if (nums.length == 1)
            return true;

        boolean [] canJumpArray = new boolean[nums.length];
        canJumpArray[nums.length-1] = true;
        for (int i=nums.length-2; i >= 0; i--) {
            int count=nums[i];
            boolean succ = false;
            if (nums[i] <= nums[i+1]+1 && canJumpArray[i+1] == false) {
                canJumpArray[i] = false;
                continue;
            } else if (nums[i] > nums[i+1] && canJumpArray[i+1] == true) {
                canJumpArray[i] = true;
                continue;
            }
            for (int j=count; j >= 1; j--) {
                if (i+j < nums.length && canJumpArray[i+j]) {
                    succ = true;
                    break;
                }
            }
            canJumpArray[i] = succ;
        }

        return canJumpArray[0];
    }
}
```
