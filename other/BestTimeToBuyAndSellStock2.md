# Best Time to Buy and Sell Stock II 
## Source
leetcode:[122 Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)

> Say you have an array for which the ith element is the price of a given stock on day i.
> 
> Design an algorithm to find the maximum profit. You may complete as many transactions 
> as you like (ie, buy one and sell one share of the stock multiple times). However, 
> you may not engage in multiple transactions at the same time (ie, you must sell 
> the stock before you buy again).

## 思路
既然能做任意次交易，那么所有低买高卖的点都把握住，获得收益最大。

只要明天的价格比今天的价格高，那么我便今天买进，第二天卖掉，获得收益为
price[i+1] - price[i]。

那如果连续两天价格上涨呢？第二天，我就不能卖了。但是这两天价格上涨的收益，我都能
得到，只要我今天买进，第三天卖掉即可。

## Java 代码
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
