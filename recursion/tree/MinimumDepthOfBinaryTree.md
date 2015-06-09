# Minimum Depth of Binary Tree
## Source
leetcode:[111 Minimum Depth of Binary Tree](https://leetcode.com/problems/minimum-depth-of-binary-tree/)
> Given a binary tree, find its minimum depth.
> 
> The minimum depth is the number of nodes along the shortest path from the root
> node down to the nearest leaf node.

## 思路
一个叶子节点要到达根节点，必定得通过它左子树的根节点或者其右子树的根节点。
所以如果一个叶子节点到达根节点的最短路径一定是其到达左子树或者右子树根节点的最短路径
再加上根节点本身。

下面的代码看起来比[Maximum Depth of Binary Tree](MaximumDepthOfBinaryTree.md)的代码
长一点,是因为空节点不认为是叶子节点。
如树
<pre>
    1
   /
  2
</pre>
的最小深度为2,而不是1，因为它只有一个叶子节点。

## Java递归解法
``` java
public class Solution {
    public int minDepth(TreeNode root) {
        if (root == null)
            return 0;
            
        if (root.left == null && root.right == null)
            return 1;
            
        if (root.left == null)
            return minDepth(root.right)+1;
        
        if (root.right == null)
            return minDepth(root.left)+1;

        return Math.min(minDepth(root.left), minDepth(root.right))+1;
    }
}
```
