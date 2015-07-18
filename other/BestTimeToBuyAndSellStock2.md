# Best Time to Buy and Sell Stock II 
## Source
leetcode:[122 Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)

> Say you have an array for which the ith element is the price of a given stock on day i.
> 
> Design an algorithm to find the maximum profit. You may complete as many transactions 
> as you like (ie, buy one and sell one share of the stock multiple times). However, 
> you may not engage in multiple transactions at the same time (ie, you must sell 
> the stock before you buy again).

## ˼·
��Ȼ��������ν��ף���ô���е�������ĵ㶼����ס������������

ֻҪ����ļ۸�Ƚ���ļ۸�ߣ���ô�ұ����������ڶ����������������Ϊ
price[i+1] - price[i]��

�������������۸������أ��ڶ��죬�ҾͲ������ˡ�����������۸����ǵ����棬�Ҷ���
�õ���ֻҪ�ҽ���������������������ɡ�

## Java ����
``` java
public class Solution {
    public int maxProfit(int[] prices) {
        if (prices == null || prices.length == 0)
            return 0;

        int profit = 0;
        for (int i=0; i < prices.length-1; i++) {
            if (prices[i+1] > prices[i])
                profit += prices[i+1]-prices[i]; 
        }
        
        return profit;
    }
}

```


