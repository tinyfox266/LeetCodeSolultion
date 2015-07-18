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

## ˼·
��Ϊ���������ν��ף���ô�����������
* ֻ����һ�ν��ף���ô������[Best Time to Buy and Sell Stock](../../other/BestTimeToBuyAndSellStock.md)��
    ��ֵ����maxBefore[N]������maxBefore[i]�ĺ������������ᵽ��NΪ���˼۸��������
    
* �������ν��ס������ν��׿��Էֱ�������[0,i)��[i,N-1]��ɣ�����NΪ���˼۸��������

����������������[0,i)����һ�ν��׵��������maxBefore[i]��ô���㡣

maxBefore[i] = Max(maxBefore[i-1], prices[i-1]-min)

���У�prices[i-1]Ϊ��i-1��ļ۸�minΪ��0�쵽��i-2��۸����Сֵ��

�÷��̵ĺ���Ϊ��
* Ҫô���һ������Ľ�����i-1ǰ���
* Ҫô�ڵ�i-1�����(��)���ܵõ����������Ϊprices[i-1]-min

Ȼ��ȡ�����������Ǹ����ɡ�

����������������[i,N-1]����һ�ν��׵��������maxAfter[i]��ô���㡣

maxAfter[i] = Max(maxAfter[i+1], max-prices[i])

���У�prices[i]��ʾ��i��ļ۸�max��ʾ��i�쵽��N-1���м۸�����ֵ��

�÷��̵ĺ���Ϊ��
* Ҫô���һ������Ľ����ڵ�i+1�쵽��N-1����ɡ�
* Ҫô���һ������Ľ��״ӵ�i�쿪ʼ(��)��

Ȼ��ȡ�����������Ǹ����ɡ�

## Java ����
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
