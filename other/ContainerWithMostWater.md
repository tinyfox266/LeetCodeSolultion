# Container With Most Water 
## Source
leetcode:[11 Container With Most Water](https://leetcode.com/problems/container-with-most-water/)
> Given n non-negative integers a1, a2, ..., an, where each represents a point at
> coordinate (i, ai). n vertical lines are drawn such that the two endpoints of
> line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis
> forms a container, such that the container contains the most water.
> 
> Note: You may not slant the container.

## 思路
容积即面积，它受长和高的影响，当长度减小时候，高必须增长才有可能提升面积，
所以我们从长度最长时开始递减，然后寻找更高的线。

如果当前遍历的container其两边分别是left和right,那下一个应该遍历(left+1,right)
呢，还是(left,
right-1)呢。应该从高度更小的那边开始缩减。如果从高度更高的那边开始缩减，缩减后
的container的面积必然比之前的小，因为面积的大小由最短的边决定，而最短的边并没有
发生变化。

## Java 代码
``` java
public class Solution {
    public int maxArea(int[] height) {
        if (height.length == 0)
            return 0;

        int left=0, right=height.length-1;
        int max=0;
        while (left<right) {
            max = Math.max(max, (right-left)*Math.min(height[left], height[right]));
            if (height[left] <= height[right]) {
                left ++;
            } else {
                right--;
            }
        }

        return max;
    }
}
```
