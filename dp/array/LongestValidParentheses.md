# Longest Valid Parentheses
## Source
leetcode:[32 Longest Valid Parentheses](https://leetcode.com/problems/longest-valid-parentheses/)
> Given a string containing just the characters '(' and ')', find the length of the longest valid (well-formed) parentheses substring.
> 
> For "(()", the longest valid parentheses substring is "()", which has length = 2.
> 
> Another example is ")()())", where the longest valid parentheses substring is "()()", which has length = 4.

## ˼·
���·�������
[ÿ���㷨֮��ʮ�ˣ�Longest Valid Parentheses](http://www.tuicool.com/articles/vUnEbi).

����Ϸ�ƥ��ĳ��ȣ�����������һά��̬�滮������⡣�����������ű��ʽΪString s��
ά��һ������Ϊs.length��һά����dp[]������Ԫ�س�ʼ��Ϊ0�� dp[i]��ʾ��s[i]��s[s.length - 1]
����s[i] �������Чƥ�������Ӵ����ȡ���������¹�ϵ��
* dp[s.length - 1] = 0;
* i��n - 2 -> 0������dp[]������¼�����ֵ����s[i] == '('������s�д�i��ʼ��
  s.length - 1����dp[i]��ֵ����������Ϊ������ͨ��dp[i + 1]���е�
 ��ע��dp[i + 1]�Ѿ�����һ����⣩��
    * ��s��Ѱ�Ҵ�i + 1��ʼ����Ч����ƥ���Ӵ����ȣ���dp[i + 1]�����������Ч�������Ӵ���
      �鿴��һ���ַ������±�Ϊj = i + 1 + dp[i + 1]����jû��Խ�磬����s[j] == ��)����
      ��s[i ... j]Ϊ��Ч����ƥ�䣬dp[i] =dp[i + 1] + 2��
    * �������s[i ... j]����Чƥ�䳤��֮����j + 1û��Խ�磬��dp[i]��ֵ��Ҫ����
      ��j + 1��ʼ�����Чƥ�䣬��dp[j + 1]��


## Java DP ����
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
