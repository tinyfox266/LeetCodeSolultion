# House Robber II 
## Source
leetcode:[213 House Robber II](https://leetcode.com/problems/house-robber-ii/)
> Note: This is an extension of [House Robber](HouseRobber.md).
> 
> After robbing those houses on that street, the thief has found himself a new
> place for his thievery so that he will not get too much attention. This time,
> all houses at this place are arranged in a circle. That means the first house is
> the neighbor of the last one. Meanwhile, the security system for these houses
> remain the same as for those in the previous street.
> 
> Given a list of non-negative integers representing the amount of money of each
> house, determine the maximum amount of money you can rob tonight without
> alerting the police.

## 思路
相比于[House Robber](HouseRobber.md), 这道题增加了一个条件，就是第0个house和
第N-1个house不能同时偷(N为house的数量)。
所以这道题的原问题0,...,i的解不能简单地由子问题0,...,i-1和
子问题0,...,i-2得到。因为在子问题0,...,i-1中，第0个house和第i-1个house不能同时被偷，
在子问题0,...,i-2中，第0个house和第i-1个house不能同时被偷。但是在原问题中，第0个house
和第i-1/i-2个house是可以同时被偷的。

把这个问题分解为两种情况，每种情况都可以转化为[House Robber](HouseRobber.md)的问题:
* 偷第0个House, 其解为从第0家偷到的钱，
    再加上按照[House Robber](HouseRobber.md)的要求偷第2,...,N-2家得到的最大金额
* 不偷第0个House,其解为按照[House Robber](HouseRobber.md)的要求偷第1,...,N-1家得到的最大金额

## Java DP 代码
``` java
public class Solution {
    public int rob(int[] nums) {
        if (nums.length == 0)
            return 0;
        if (nums.length == 1)
            return nums[0];
            
        if (nums.length == 2)
            return Math.max(nums[0], nums[1]);
        return Math.max(rob12(Arrays.copyOfRange(nums, 2, nums.length - 1)) + nums[0],
                rob12(Arrays.copyOfRange(nums, 1, nums.length )));
    }

    public int rob12(int [] nums) {
        if (nums.length <= 0)
            return 0;

        if (nums.length == 1)
            return nums[0];
        int [] maxStolen = new int[nums.length];
        maxStolen[0] = nums[0];
        maxStolen[1] = Math.max(maxStolen[0], nums[1]);
        if (nums.length == 2)
            return maxStolen[1];
        for (int i=2; i < nums.length; i++) {
            maxStolen[i] = Math.max(maxStolen[i-1], maxStolen[i-2]+nums[i]);
        }
        return maxStolen[nums.length-1];
    }
}
```
