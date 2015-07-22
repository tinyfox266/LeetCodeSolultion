# Gas Station
## Source
leetcode:[134 Gas Station](https://leetcode.com/problems/candy/)
> There are N gas stations along a circular route, where the amount of gas at
> station i is gas[i].
> 
> You have a car with an unlimited gas tank and it costs cost[i] of gas to travel
> from station i to its next station (i+1). You begin the journey with an empty
> tank at one of the gas stations.
> 
> Return the starting gas station's index if you can travel around the circuit
> once, otherwise return -1.
> 
> **Note**:
> 
> The solution is guaranteed to be unique.

## 思路
假设从第i个Station出发，它能够重新回到出发点，那么需要满足以下条件：
* 车子走到之后的每个车站，所剩油量都是正的。

所以从第1个车站后开始依次累加加油量与消耗量的差值，当第一次累加值为负时
(比如第i个车站)， 说明前i-1个车站都不能作为始发车站:
* 第1个车站不能作为始发，因为从它开始，连第i个车站都到大不了。
* 第2,...,i-1个车站也不行i(不妨将其设为j),是因为从第1个车站到第i个车站的累加值为
    负,而第1个车站到第j个车站的累加值为正，所以第j个车站到第i车站的累加值一定为负。
    
然后从第i+1个车站开始重新累加。直到从某个车站开始，累加值始终为正。最后会出现两种
情况：
* 找不到累加值始终为正的车站，那么不存在能够绕行一圈的始发车站。这是因为，所有的
    始发车站，我们都遍历了一遍，发现都不合适。
* 找到一个车站，其后的累加值都是为正。那么这个车站为候选车站，然后再验证从其出发是否
    真的绕行一圈即可。

## Java 代码
``` java
public class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        if (gas.length == 0)
            return -1;
        if (gas.length != cost.length)
            return -1;

        int index=0;
        int sum=0;
        for (int i=0; i < gas.length; i++) {
            sum += gas[i]-cost[i];
            if (sum < 0) {
                index=i+1;
                sum=0;
            }
        }
        if (index == gas.length)
            return -1;
        for (int i=0; i < index; i++) {
            sum += gas[i] - cost[i];
            if (sum < 0)
                return -1;
        }
        return index;
    }
}
```
