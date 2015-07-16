# Trapping Rain Water 
## Source 
leetcode:[42 Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)
> Given n non-negative integers representing an elevation map where the width of
> each bar is 1, compute how much water it is able to trap after raining.
> 
> For example, 
>
> Given [0,1,0,2,1,0,1,3,2,1,2,1], return 6.
> 
> ![rain water trap](images/rainwatertrap.png)
> 
> The above elevation map is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In
> this case, 6 units of rain water (blue section) are being trapped. Thanks Marcos
> for contributing this image!

## 思路
第i个位置能存水的条件是: 左右两边都要有比它高的柱。
它能储水的量为其左边的最高柱和其右边的最高柱中小的那个的高度减去其第i个柱自己的高度
(所有桶的容量由最短的那块板决定)。

假设left[i]为在i左边最高柱的高度，right[i]为在i右边最高柱的高度，第i个位置的高度为
height[i]，则第i个位置能存水的量为Max(Min(left[i], right[i]) - height[i], 0).

整个区域能储水的最大值为每个位置能储水的最大值的和。 


## Java 代码
``` java
public class Solution {
    public int trap(int[] height) {
        if (height == null || height.length == 0)
            return 0;
        if (height.length == 1)
            return 0;

        int sum=0;
        int [] left = new int[height.length];
        int [] right = new int[height.length];
        int max=0;
        for (int i=0; i < height.length; i++) {
            left[i] = Math.max(max, height[i]);
            max = left[i];
        }

        max = 0;
        for (int i=height.length-1; i >= 0; i--) {
            right[i] = Math.max(max, height[i]);
            max = right[i];
        }

        for (int i=0; i < height.length; i++) {
            sum += Math.max((Math.min(left[i],right[i]) - height[i]), 0);
        }

        return sum;
    }
}

```

