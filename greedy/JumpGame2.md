# Jump Game II
## Source
leetcode:[55 Jump Game](https://leetcode.com/problems/jump-game-ii/)
> Given an array of non-negative integers, you are initially positioned at the
> first index of the array.
> 
> Each element in the array represents your maximum jump length at that position.
> 
> Your goal is to reach the last index in the minimum number of jumps.
> 
> For example:
>
> Given array A = [2,3,1,1,4]
> 
> The minimum number of jumps to reach the last index is 2. (Jump 1 step from
> index 0 to 1, then 3 steps to the last index.)

## 思路
使用常规的动态规划的办法，不能通过leetcode OJ的大数据（超时）。

下面介绍一种贪心策略，其时间复杂度为O(n).

从左向右遍历数组。当遍历到第i个元素的时候，我们知道了从i能够到达的位置为i+1,...,i+nums[i]，其中
nums[i]表示从i位置能够跳的最大步数。于是从0到i+1,...,i+nums[i]的最小步数为到i的最小步数 
(不妨记为ret)+1。
会有两种情况：
* 最后一个位置N包含在i+1,...,i+nums[i]中，那么从0到达N的最小步数就找到了。如果还没到达N，
* 否则，计算从i+1,...,i+nums[i]能够到达的最大位置(不放记为max)：
    * 如果这个最大位置小于等于i+nums[i],那么从0不可能到达最后一个位置
    * 否则当遍历到i+nums[i]+1,...,max时，设ret为ret+1。表示能够到达i+nums[i],...,max
        的最下步数为ret+1。

## Java Greedy 代码
``` java
public class Solution {
    public int jump(int[] nums) {
        if (nums == null || nums.length == 0)
            return 0;
        if (nums.length == 1)
            return 0;

        int curMax=0;
        int curRch=0;
        int ret=0;
        for (int i=0; i < nums.length; i++) {
            if (curRch < i) {
                ret++;
                curRch = curMax;
            }
            curMax = Math.max(curMax, nums[i]+i);
        }
        
        return ret;
    }
}
```
