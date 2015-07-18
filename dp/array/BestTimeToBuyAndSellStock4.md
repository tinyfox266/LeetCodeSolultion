# Best Time to Buy and Sell Stock IV 
## Source
leetcode:[123 Best Time to Buy and Sell Stock III](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/)
> Say you have an array for which the ith element is the price of a given stock on day i.
> 
> Design an algorithm to find the maximum profit. You may complete at most k transactions.
> 
> **Note**:
> You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

## ˼·
�����[Best Time to Buy and Sell Stock II](../../other/BestTimeToBuyAndSellStock2.md),
����Խ��׵Ĵ����������ơ���������������������ۣ�
* k > N/2: �������N��ļ۸���ô����ܽ���N/2�ν��ף����k>N/2����ô�൱�ڶ�
    ���׵Ĵ���û�����ƣ�����ת��Ϊ��
    [Best Time to Buy and Sell Stock II](../../other/BestTimeToBuyAndSellStock2.md)
* k <= N/2.

����������k <= N/2�������

�����������������ݽṹ�������뵽�ö�ά��̬�滮����⣬���е�(i,j)��Ԫ��`dp[i][j]`
��ʾ����i��ļ۸񣬽��״�������Ϊj��������档


��ֱ����`dp[i-1][j]`��`dp[i][j-1]`��`dp[i-1][j-1]`����õ�`dp[i][j]`��
�Ⲣ�����ס��������ǰ�`dp[i][j]`��������������ۣ�
* ��i�첻������Ʊ����ʱ�����������Ϊǰi-1�죨��������������j�ε�������档
* ��i��������Ʊ: ��ʱ�����������Ϊǰi-1�죨��������������j-1�ε�������档����
    ���ν��ף�����j�Ρ����������ǰi-1�죨������j-1�ν����У������һ�������˹�Ʊ��
    ��ôj��Ͳ���������Ʊ,��ǰ��ì���ˡ�

��Ȼ����i���Ƿ���������Ʊ���i-1���Ƿ�������Ʊ�йأ������ǲ�����¼���������
��i��������Ʊ���������`lastDaySell[i][j]`�͵�i�첻������Ʊ���������`lastDayNotSell[i][j]`��
���ǣ�������������Ķ��淽�̣�

`lastDaySell[i][j] = Max(lastDayNotSell[i-1][j-1], lastDaySell[i-1][j]) + prices[i] - prices[i-1]`

`lastDayNotSell[i][j] = Max(lastDayNotSell[i-1][j], lastDaySell[i-1][j])`

���У�prices[i]��ʾ��i��ļ۸�

��i���������Ʊ���ҽ�������j�ε��������`lastDaySell[i][j]`Ϊ�������ߵ����ֵ��
* ��i-1�첻������Ʊ���佻������j-1�ε��������`lastDayNotSell[i-1][j-1]`
     ���ϵ�i��������Ʊ������`prices[i]-prices[i-1]`��
* ��i-1��������Ʊ���佻������j�ε��������`lastDaySell[i-1][j]`
    ���ϵ�i��������Ʊ������`prices[i]-prices[i-1]`��
    ��ʱ��ǰi-1������һ�ν��׺͵�i��������Ʊ����ν��׺ϲ��ˡ�


��i�첻������Ʊ���ҽ�������j�ε��������`lastDayNotSell[i][j]`Ϊ�������ߵ����ֵ��
* ��i-1�첻������Ʊ���佻������j�ε��������`lastDayNotSell[i-1][j]`��
* ��i-1��������Ʊ���佻������j�ε��������`lastDaySell[i-1][j]`��

## Java DP ����
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
