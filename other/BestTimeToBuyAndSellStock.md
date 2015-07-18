# Best Time to Buy and Sell Stock
## Source
leetcode:[121 Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)
> Say you have an array for which the ith element is the price of a given stock on day i.
> 
> If you were only permitted to complete at most one transaction (ie, buy one and sell one share of the stock), design an algorithm to find the maximum profit.

## 思路
首先得到如果第i天卖的话，能够获得的最大收益maxSell[i]，然后取这N个值中的最大值。

maxSell[i] = prices[i] - min

其中，prices[i]表示第i天的价格， min表示第0,...,i-1天中价格的最小值。

## Java 代码
``` java
public class Solution {
    public int maxProfit(int[] prices) {
        if (prices == null || prices.length == 0 || prices.length == 1)
            return 0;
        if (prices.length == 2){
            if (prices[0] > prices[1])
                return 0;
            return prices[1] - prices[0];
        }

        int min = Integer.MAX_VALUE;
        int max = 0;

        for(int p:prices) {
            min = Math.min(min, p);
            max = Math.max(p-min, max);
        }

        return max;
    }
}
```
