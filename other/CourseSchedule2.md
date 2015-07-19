# Course Schedule II
## Source
leetcode:[2l0 Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)
> There are a total of n courses you have to take, labeled from 0 to n - 1.
> 
> Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: [0,1]
> 
> Given the total number of courses and a list of prerequisite pairs, return the ordering of courses you should take to finish all courses.
> 
> There may be multiple correct orders, you just need to return one of them. If it is impossible to finish all courses, return an empty array.
> 
> For example:
> 
> 2, [[1,0]]
> 
> There are a total of 2 courses to take. To take course 1 you should have finished course 0. So the correct course order is [0,1]
> 
> 4, [[1,0],[2,0],[3,1],[3,2]]
> 
> There are a total of 4 courses to take. To take course 3 you should have finished both courses 1 and 2. Both courses 1 and 2 should be taken after you finished course 0. So one correct course order is [0,1,2,3]. Another correct ordering is[0,2,1,3].
> 
> **Note**:
> The input prerequisites is a graph represented by a list of edges, not adjacency matrices. 
> Read more about [how a graph is represented](https://www.khanacademy.org/computing/computer-science/algorithms/graph-representation/a/representing-graphs).

## 思路
相比于[Course Schedule](CourseSchedule.md),这道题如果存在满足要求的课程安排的话，
输出其中的一种安排顺序。我需要增加的操作就是在每当删除一个没有前继的节点时，便
将这个节点加入到结果的数组order中。

## Java 代码
``` java
public class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
         if (numCourses <= 0)
            return new int[0];
        Set<Integer> noPreSet = new HashSet<Integer>();
        Map<Integer, List<Integer>> preMap = new HashMap<Integer, List<Integer>>();
        int [] dependCount = new int [numCourses];
        int [] order = new int[numCourses];
        int index=0;
        Arrays.fill(dependCount, 0);
        for (int i=0; i < numCourses; i++) {
            noPreSet.add(i);
            preMap.put(i, new LinkedList<Integer>());
        }

        for (int i=0; i < prerequisites.length; i++) {
            int left = prerequisites[i][0];
            int right = prerequisites[i][1];
            preMap.get(right).add(left);
            dependCount[left]++;
            noPreSet.remove(left);
        }

        while (!noPreSet.isEmpty()) {
            List<Integer> noPreSetCache = new LinkedList<Integer>(noPreSet);
            for (int c: noPreSetCache) {
                order[index++] = c;
                noPreSet.remove(c);
                for (int pre: preMap.get(c)) {
                    dependCount[pre]--;
                    if (dependCount[pre] == 0)
                        noPreSet.add(pre);
                }
            }
        }

        for (int i=0; i < numCourses; i++) {
            if (dependCount[i] != 0)
                return new int[0];
        }

        return order;       
    }
}
```

