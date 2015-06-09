# Maximum Depth of Binary Tree
## Source 
leetcode:[104 Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)
> Given a binary tree, find its maximum depth.
> 
> The maximum depth is the number of nodes along the longest path from the root
> node down to the farthest leaf node

## 思路
原问题可以被分解为两个子问题。一棵树由根节点，左子树和右子树构成。假设左子树和右子树
的最大深度分别为lMax和rMax，那么这棵树的最大深度为Max(lMax,rMax)+1。最小的子问题为只有
一个节点的树，其最大深度为1,或者是根本没有节点的树，其最大深度为0。


## Java递归解法
``` java
public class Solution {
    public int maxDepth(TreeNode root) {
       if (root == null)
           return 0;
        return (Math.max(maxDepth(root.left),maxDepth(root.right)) + 1);
    }
}
```
