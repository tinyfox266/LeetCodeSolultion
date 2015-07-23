# Longest Valid Parentheses
## Source
leetcode:[32 Longest Valid Parentheses](https://leetcode.com/problems/longest-valid-parentheses/)
> Given a string containing just the characters '(' and ')', find the length of the longest valid (well-formed) parentheses substring.
> 
> For "(()", the longest valid parentheses substring is "()", which has length = 2.
> 
> Another example is ")()())", where the longest valid parentheses substring is "()()", which has length = 4.

## 思路
以下分析来自
[每日算法之二十八：Longest Valid Parentheses](http://www.tuicool.com/articles/vUnEbi).

求最长合法匹配的长度，这道题可以用一维动态规划逆向求解。假设输入括号表达式为String s，
维护一个长度为s.length的一维数组dp[]，数组元素初始化为0。 dp[i]表示从s[i]到s[s.length - 1]
包含s[i] 的最长的有效匹配括号子串长度。则存在如下关系：
* dp[s.length - 1] = 0;
* i从n - 2 -> 0逆向求dp[]，并记录其最大值。若s[i] == '('，则在s中从i开始到
  s.length - 1计算dp[i]的值。这个计算分为两步，通过dp[i + 1]进行的
 （注意dp[i + 1]已经在上一步求解）：
    * 在s中寻找从i + 1开始的有效括号匹配子串长度，即dp[i + 1]，跳过这段有效的括号子串，
      查看下一个字符，其下标为j = i + 1 + dp[i + 1]。若j没有越界，并且s[j] == ‘)’，
      则s[i ... j]为有效括号匹配，dp[i] =dp[i + 1] + 2。
    * 在求得了s[i ... j]的有效匹配长度之后，若j + 1没有越界，则dp[i]的值还要加上
      从j + 1开始的最长有效匹配，即dp[j + 1]。


## Java DP 代码
``` java        
public class Solution {
    public int longestValidParentheses(String s) {
        if (s == null || s.length() == 0)
            return 0;

        int [] dp = new int[s.length()];

        Arrays.fill(dp, 0);
        int max=0;
        for (int i=s.length()-2; i >= 0; i--) {
            char c = s.charAt(i);
            if (c == '(') {
                int j = i+dp[i+1]+1;
                if (j < s.length() && s.charAt(j) == ')') {
                    dp[i] = dp[i+1] + 2;
                    if (j+1 < s.length()) {
                       dp[i] += dp[j+1];
                    }
                }

                max = Math.max(max, dp[i]);
            }
        }

        return max;
    }
}
```
