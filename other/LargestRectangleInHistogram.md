# Largest Rectangle in Histogram
## Source
leetcode:[84 Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/)
> Given n non-negative integers representing the histogram's bar height where the
> width of each bar is 1, find the area of largest rectangle in the histogram.
> 
> ![histogram](images/histogram.png)
> 
> Above is a histogram where width of each bar is 1, given height = [2,1,5,6,2,3].
> 
> ![histogram_area](images/histogram_area.png)
> 
> The largest rectangle is shown in the shaded area, which has area = 10 unit.
> 
> For example,
>
> Given height = [2,1,5,6,2,3],
>
> return 10.

## 思路
### 思路一
我们可以先求完全包含第i个柱(bar)的最大矩形，然后选出这i个矩形中最大的那个即可。
下面我们来分析包含第i个柱的最大矩形如何来求。

既然是完全包含第i个柱，那么这个矩形的高度就是这个柱的高度height[i]。假设这个矩形的跨度是
从m到n，那么从m到n这n-m+1个柱的高度必须都大于等于height[i]。于是，对于最大的那个矩形有
m == 0 或者height[m-1] < height[i], n == height.length-1 或者 n <
height[i]。所以，我们只要找到i左右两边第一个高度比height[i]小的柱即可,分别记录为right[i]和
left[i]。


最直接的办法就是对每个柱i,都这样搜索一遍，时间复杂度是O(n^2),每个柱都被扫描了n遍。其实，只要将扫描的
历史记录下来，就可以做到只扫描一遍即可。

首先，我们来分析如何只做一次扫描就得到每个柱的右边界。

用一个Stack来记录还没有找到右边界的柱的序号i，当扫描到的当前柱cur的高度比栈顶柱top的高度低，那么第top个柱
找到了其右边界,为cur-1,将top出站，然后重新扫描cur。当cur的高度不大于top的高度，那么将cur压栈。


用类似的方法,从右边开始扫描，可以得到所有柱的左边界。

有了left[i]和right[i]后，每个候选矩形的面积为height[i] * (right[i]-left[i]+1),找出其中最大的那个就可以了。

其代码为Java 代码一

### 思路改进
按照上面的代码，过不了leetcode大数据。稍加改进即可在求出right[i]的同时，得到left[i]。

同样地，用一个Stack来记录还没有找到右边界的柱的序号i，当扫描到的当前柱cur的高度比栈顶柱top的高度低，那么第top个柱
找到了其右边界,为cur-1.  此时stack中的柱的高度必定都小于等于top的高度。假设在stack中离top最近，
且高度比top小的柱为j, 那么left[top]为j+1, 这是因为left[j]既然还在stack中，说明height[j+1...cur]还没有一个元素比height[j]小。

改进后的代码见Java  代码二




## 代码
## Java 代码一
``` java
public class Solution {
    public int largestRectangleArea(int[] height) {
        if (height == null || height.length == 0)
            return 0;

        Stack<Integer> stack = new Stack<Integer>();
        int [] left = new int [height.length];
        int [] right = new int [height.length];
        left[0] = 0;
        right[height.length-1] = height.length-1;


        for (int i=0; i < height.length; i++) {
            if (stack.isEmpty() || height[stack.peek()] <= height[i]) {
                stack.push(i);
            } else {
                int top = stack.pop();
                right[top] = i-1;
                i--;
            }
        }

        while (!stack.isEmpty()) {
            int top = stack.pop();
            right[top] = height.length-1;
        }

        stack.clear();

        for (int i=height.length-1; i >= 0; i--) {
            if (stack.isEmpty() || height[stack.peek()] <= height[i]) {
                stack.push(i);
            } else {
                int top = stack.pop();
                left[top] = i+1;
                i++;
            }
        }

        while (!stack.isEmpty()) {
            int top = stack.pop();
            left[top] = 0;
        }

        int max = 0;
        for (int i=0; i < height.length; i++) {
            max = Math.max(max, (right[i]-left[i]+1) * height[i]);
        }

        return max;
    }
}
```
## Java 代码二
``` java
public class Solution {
    public int largestRectangleArea(int[] height) {
        if (height == null || height.length == 0)
            return 0;

        Stack<Integer> stack = new Stack<Integer>();

        int max=0;

        for (int i=0; i < height.length; i++) {
            if (stack.isEmpty() || height[stack.peek()] <= height[i]) {
                stack.push(i);
            } else {
                int top = stack.pop();
                while (!stack.isEmpty() && stack.peek() == top) {
                    stack.pop();
                }
                int start = stack.isEmpty()?0:stack.peek()+1;
                max = Math.max(max, (i-start) * height[top]);
                i--;

            }
        }

        while (!stack.isEmpty()) {
            int top = stack.pop();
            while (!stack.isEmpty() && stack.peek() == top) {
                stack.pop();
            }
            int start = stack.isEmpty()?0:stack.peek()+1;
            max = Math.max(max, (height.length-start) * height[top]);
        }

        return max;
    }
}
````

