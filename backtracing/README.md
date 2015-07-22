回溯法可以看做是BruteForce方法的一点改进，当发现当前尝试的解
的一部分不满足条件时，就立刻尝试下一个解。其代码框架为
``` java
backtracing(c) {
    if reject(P,c) then return
    if accept(P,c) then output(P,c)
    s <- first(P,c)
    while s != null {
        backtracing(s)
        s <- next(P,s)
    }
}
```
其中，
* `c`是一个候选解
* `P`是数据集合
* `reject(P,c)` 表示c不是一个解，此时返回。
* `accept(P,c)` 表示c是一个解，此时将结果保存。
* `first(P,c)`表示c的一个扩展。
* `next(P,s)`表示c的除s之外的另外一个扩展。
