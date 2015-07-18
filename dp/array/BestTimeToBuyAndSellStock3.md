# Best Time to Buy and Sell Stock III
## Source
leetcode:[123 Best Time to Buy and Sell Stock III](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/)

> Say you have an array for which the ith element is the price of a given stock on day i.
> 
> Design an algorithm to find the maximum profit. You may complete at most two transactions.
> 
> **Note**:
> 
> You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

## 思路
因为至多做两次交易，那么有两种情况：
* 只做了一次交易，那么便变成了[Best Time to Buy and Sell Stock](../../other/BestTimeToBuyAndSellStock.md)。
    其值就是maxBefore[N]，其中maxBefore[i]的含义在下文中提到，N为给了价格的天数。
    
* 做了两次交易。这两次交易可以分别在区间[0,i)和[i,N-1]完成，其中N为给了价格的天数。

下面来看，在区间[0,i)内做一次交易的最大收益maxBefore[i]怎么计算。

maxBefore[i] = Max(maxBefore[i-1], prices[i-1]-min)

其中，prices[i-1]为第i-1天的价格，min为第0天到第i-2天价格的最小值。

该方程的含义为：
* 要么最大一次收益的交易在i-1前完成
* 要么在第i-1天完成(卖)，能得到的最大收益为prices[i-1]-min

然后取两者中最大的那个即可。

接着来看，在区间[i,N-1]内做一次交易的最大收益maxAfter[i]怎么计算。

maxAfter[i] = Max(maxAfter[i+1], max-prices[i])

其中，prices[i]表示第i天的价格，max表示第i天到第N-1天中价格的最大值。

该方程的含义为：
* 要么最大一次收益的交易在第i+1天到第N-1天完成。
* 要么最大一次收益的交易从第i天开始(买)。

然后取两者中最大的那个即可。

## Java 代码
```
public class Solution {
    public int maxProfit(int[] prices) {
        if (prices == null || prices.length == 0 || prices.length == 1)
            return 0;

        int [] maxBefore = new int[prices.length+1];
        maxBefore[0] = 0;

        int min=prices[0];
        for (int i=1; i <= prices.length; i++) {
            min = Math.min(min, prices[i-1]);
            maxBefore[i] = Math.max(maxBefore[i-1], prices[i-1]-min);
        }

        int max=prices[prices.length-1];
        int ret = maxBefore[prices.length];
        for (int i=prices.length-1; i >= 0; i--) {
            max = Math.max(max, prices[i]);
            ret = Math.max(ret, maxBefore[i] + max - prices[i]);
        }

        return ret;
    }
}

```
