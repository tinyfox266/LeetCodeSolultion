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
* 车子走到之后的每个车站，所剩油量都是正的。也就是
    $for i.\sum_{j=i}^{i-1}(gas[i]-cost[i])>0$.



## Java 代码
