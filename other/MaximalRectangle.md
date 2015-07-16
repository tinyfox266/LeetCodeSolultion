# Maximal Rectangle 
## Source
leetcode:[85 Maximal Rectangle](https://leetcode.com/problems/maximal-rectangle/)
> Given a 2D binary matrix filled with 0's and 1's, find the largest rectangle
> containing all ones and return its area.

## 思路
这道题可以转化为问题[84 Largest Rectangle in Histogram](LargestRectangleInHistogram.md)

将matrix中的每一行元素依次累加，得到一个新的矩阵，然后对其每一行运行
[84 Largest Rectangle in Histogram](LargestRectangleInHistogram.md)的解法,
找到N个返回值中最大的那个即可。其中N为原matrix的行数。

比如原matrix为
<pre>
1 0 0 0 
1 1 0 0
1 1 1 0
1 1 1 1
</pre>
则新的matrix为
<pre>
1 0 0 0
2 1 0 0
3 2 1 0
4 3 2 1
</pre>
## Java 代码
``` Java 
public class Solution {
    public int maximalRectangle(char[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0)
            return 0;

        int [][] intMatrix = new int[matrix.length][matrix[0].length];

        for (int j=0; j < matrix[0].length; j++) {
            intMatrix[0][j] = matrix[0][j]-'0';
        }

        for (int i=1; i < matrix.length; i++) {
            for (int j=0; j < matrix[0].length; j++) {
                intMatrix[i][j] = (matrix[i][j]=='1' ? intMatrix[i-1][j]+1 : 0);
            }
        }

        int max=0;
        for (int i=0; i < intMatrix.length; i++) {
            max = Math.max(max, largestRectangleArea(intMatrix[i]));
        }

        return max;
    }

    int largestRectangleArea(int [] height) {
        Stack<Integer> stack = new Stack<Integer>();

        int max=0;

        for (int i=0; i < height.length; i++) {
            if (stack.isEmpty() || height[stack.peek()] <= height[i])
                stack.push(i);
            else {
                int e = stack.pop();

                if (!stack.isEmpty()) {
                    max = Math.max(max, (i - stack.peek() - 1) * height[e]);
                } else {
                    max = Math.max(max, i*height[e]);
                }
                i--;
            }
        }

        while (! stack.isEmpty()) {
            int e = stack.pop();
            if (!stack.isEmpty()) {
                max = Math.max(max, (height.length - stack.peek() - 1) * height[e]);
            } else {
                max = Math.max(max, (height.length ) * height[e]);
            }
        }

        return max;
    }
}
```
