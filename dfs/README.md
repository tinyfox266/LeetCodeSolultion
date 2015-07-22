当遍历一个数据结构的时候，如数组、树、图等，深度优先遍历
是一种常用的遍历方式。这种遍历方式的好处是可以很容易地写出
递归算法。

下面阐述了一些数据结构深度优先遍历的基本框架。
* 二维数组
    ``` java
    dfs(int [][] matrix, int i, int j) {
        if matrix[i][j] has been visited, then return
        if i or j is beyond the scope, then return 
        
        mark matrix[i][j] as visited  
        
        do something
        
        dfs(i-1, j)
        dfs(i+1, j)
        dfs(i, j-1)
        dfs(i, j+1)
    }
    ```
    
* 二叉树
    ``` java
    dfs(TreeNode root) {
        if root is null or has been visited, then return
        
        mark root as visited
        
        do something
        
        dfs(root.left)
        dfs(root.right)
    }
    ```
