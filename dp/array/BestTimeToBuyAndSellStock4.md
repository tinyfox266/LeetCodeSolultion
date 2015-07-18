# Best Time to Buy and Sell Stock IV 
## Source
leetcode:[123 Best Time to Buy and Sell Stock III](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/)
> Say you have an array for which the ith element is the price of a given stock on day i.
> 
> Design an algorithm to find the maximum profit. You may complete at most k transactions.
> 
> **Note**:
> You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

## 思路
相比于[Best Time to Buy and Sell Stock II](../../other/BestTimeToBuyAndSellStock2.md),
这里对交易的次数做了限制。我们来分两种情况来讨论：
* k > N/2: 如果给了N天的价格，那么最多能进行N/2次交易，如果k>N/2，那么相当于对
    交易的次数没有限制，问题转换为了
    [Best Time to Buy and Sell Stock II](../../other/BestTimeToBuyAndSellStock2.md)
* k <= N/2.

下面来讨论k <= N/2的情况。

看到数组这样的数据结构，容易想到用二维动态规划来求解，其中第(i,j)个元素`dp[i][j]`
表示给了i天的价格，交易次数限制为j的最大收益。


能直接由`dp[i-1][j]`、`dp[i][j-1]`和`dp[i-1][j-1]`计算得到`dp[i][j]`吗？
这并不容易。假设我们把`dp[i][j]`分两种情况来讨论：
* 第i天不卖出股票：此时，其最大收益为前i-1天（含）交易了至多j次的最大收益。
* 第i天卖出股票: 此时，其最大收益为前i-1天（含）交易了至多j-1次的最大收益。加上
    本次交易，正好j次。可是如果在前i-1天（含）的j-1次交易中，在最后一天卖出了股票。
    那么j天就不能卖出股票,与前提矛盾了。

既然，第i天是否能卖出股票与第i-1天是否卖出股票有关，那我们不妨记录两种情况，
第i天卖出股票的最大收益`lastDaySell[i][j]`和第i天不卖出股票的最大收益`lastDayNotSell[i][j]`。
于是，我们有了下面的动规方程：

`lastDaySell[i][j] = Max(lastDayNotSell[i-1][j-1], lastDaySell[i-1][j]) + prices[i] - prices[i-1]`

`lastDayNotSell[i][j] = Max(lastDayNotSell[i-1][j], lastDaySell[i-1][j])`

其中，prices[i]表示第i天的价格。

第i天必须卖股票，且交易至多j次的最大收益`lastDaySell[i][j]`为下面两者的最大值：
* 第i-1天不卖出股票，其交易至多j-1次的最大收益`lastDayNotSell[i-1][j-1]`
     加上第i天卖出股票的收益`prices[i]-prices[i-1]`。
* 第i-1天卖出股票，其交易至多j次的最大收益`lastDaySell[i-1][j]`
    加上第i天卖出股票的收益`prices[i]-prices[i-1]`。
    此时，前i-1天的最后一次交易和第i天卖出股票的这次交易合并了。


第i天不卖出股票，且交易至多j次的最大收益`lastDayNotSell[i][j]`为下面两者的最大值：
* 第i-1天不卖出股票，其交易至多j次的最大收益`lastDayNotSell[i-1][j]`。
* 第i-1天卖出股票，其交易至多j次的最大收益`lastDaySell[i-1][j]`。

## Java DP 代码
``` java
public class Solution {
    public int maxProfit(int k, int[] prices) {
        if (prices == null || prices.length < 2)
            return 0;

        if (k <= 0)
            return 0;

        if (k > prices.length/2)
            return kIsInfinite(prices);

        int [][] lastDaySell = new int[prices.length][k+1];
        int [][] lastDayNotSell = new int[prices.length][k+1];

        Arrays.fill(lastDaySell[0], 0);
        Arrays.fill(lastDayNotSell[0], 0);
        for (int i=0; i < prices.length-1; i++) {
            lastDaySell[i][0] = 0;
            lastDayNotSell[i][0] = 0;
        }

        for (int i=1; i < prices.length; i++) {
            int diff = prices[i] - prices[i-1];
            for (int j=1; j < k+1; j++) {
                lastDaySell[i][j] = Math.max(
                        lastDayNotSell[i-1][j-1] + diff,
                        lastDaySell[i-1][j] + diff);

                lastDayNotSell[i][j] = Math.max(
                        lastDayNotSell[i-1][j],
                        lastDaySell[i-1][j]);
            }
        }

        return Math.max(lastDaySell[prices.length-1][k], lastDayNotSell[prices.length-1][k]);
    }

    public int kIsInfinite(int[] prices) {
        int profit = 0;
        for (int i=1 ; i < prices.length; i++) {
            if (prices[i] > prices[i-1]) {
                profit += prices[i] - prices[i-1];
            }
        }

        return profit;
    }
}

```
