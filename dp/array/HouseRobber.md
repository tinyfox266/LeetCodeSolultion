# House Robber
## Source
leetcode:[198 House Robber](https://leetcode.com/problems/house-robber/)
> You are a professional robber planning to rob houses along a street. Each
> house i
> has a certain amount of money nums[i] stashed, the only constraint stopping you from
> robbing each of them is that adjacent houses have security system connected and
> it will automatically contact the police if two adjacent houses were broken into
> on the same night.
> 
> Given a list of non-negative integers representing the amount of money of each
> house, determine the maximum amount of money you can rob tonight without
> alerting the police.

## 思路
动态规划方程为:

`dp[i] = Max(dp[i-1], dp[i-2]+ nums[i])`

其中, `dp[i]`表示如果只偷第0,...,i家，小偷能偷到的最大金额。

有两种情况：
* 偷第i家，那么第i-1家便不能偷，所以他能获得最大收益为偷0,...,i-2家的最大收益，
  加上第i家的收益。 
* 不偷第i家，那么他能获得的最大收益是偷0,...,i-1家的最大收益。  

每次都去上述两种情况中大的那个即可。

边界条件dp[0] = nums[0]。因为只有一家可偷，当然选择偷啦。



## Java DP 代码
``` Java
public class Solution {
    public int rob(int[] nums) {
        if (nums.length <= 0)
            return 0;
        int [] robbedHouse = new int[nums.length+1];
        robbedHouse[1] = nums[0];
        robbedHouse[0] = 0;
        for (int i=1; i < nums.length; i++) {
            robbedHouse[i+1] = Math.max(robbedHouse[i-1]+nums[i], robbedHouse[i]);
        }

        return robbedHouse[nums.length];

    }
}
```
