# Candy
## Source
leetcode:[135 Candy](https://leetcode.com/problems/candy/)
> There are N children standing in a line. Each child is assigned a rating value.
> 
> You are giving candies to these children subjected to the following
> requirements:
> * Each child must have at least one candy.
> * Children with a higher rating get more candies than their neighbors.
>  
> What is the minimum candies you must give?

## 思路
先从从向右遍历一遍所有孩子，计算满足下面条件，需要给每个孩子的最少蜡烛：
* 如果一个孩子的分数比左边孩子的分数高，那么他拥有的蜡烛数比左边的孩子多。

所以有：
* 如果一个孩子的分数比左边孩子的分数高,那么他拥有的蜡烛数比左边的孩子多1。
* 否则，其拥有的蜡烛数为1。


然后从右向左遍历一遍所有孩子，调整孩子的蜡烛数，使得满足下面的条件：
* 如果一个孩子的分数比右边孩子的分数多，那么他所拥有的蜡烛数要比右边的孩子多。

于是有：
* 如果一个孩子的分数比右边孩子的分数多，那么他拥有的蜡烛数有两种情况：
    * 如果他的分数比左边孩子的分数多，那么他拥有的蜡烛比两边的孩子都要多，所以比
        两边孩子蜡烛数的最大值多1.
    * 否则，他的蜡烛数比右边孩子的蜡烛数多1
* 否则,他的蜡烛数不变。

把所有孩子能拥有的最小蜡烛数累加，即是最后结果。

## Java 代码
``` java
public class Solution {
    public int candy(int[] ratings) {
        if (ratings == null || ratings.#length == 0)
            return 0;

        int [] candy = new int[ratings.length];

        Arrays.fill(candy, 1);
        for (int i=1; i < candy.length; i++) {
            if (ratings[i] > ratings[i-1])
                candy[i] = candy[i-1]+1;
        }

        for (int i=candy.length-2; i >=0; i--) {
            if (ratings[i] > ratings[i+1]) {
                if (i > 0 && ratings[i-1] < ratings[i]) {
                    candy[i] = Math.max(candy[i-1], candy[i+1])+1;
                } else {
                    candy[i] = candy[i+1]+1;
                }
            }
        }

        int sum = 0;
        for (int i=0; i < candy.length; i++) {
            sum += candy[i];
        }

        return sum;
    }
}
```
