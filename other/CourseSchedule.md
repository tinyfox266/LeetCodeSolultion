# Course Schedule
## Source
leetcode:[207 Course Schedule](https://leetcode.com/problems/course-schedule/).
> There are a total of n courses you have to take, labeled from 0 to n - 1.
> 
> Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: [0,1]
> 
> Given the total number of courses and a list of prerequisite pairs, is it possible for you to finish all courses?
> 
> For example:
> 
> 2, [[1,0]]
> 
> There are a total of 2 courses to take. To take course 1 you should have finished course 0. So it is possible.
> 
> 2, [[1,0],[0,1]]
> 
> There are a total of 2 courses to take. To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible.
> 
> **Note**:
> The input prerequisites is a graph represented by a list of edges, not adjacency matrices. Read more about 
> [how a graph is represented](https://www.khanacademy.org/computing/computer-science/algorithms/graph-representation/a/representing-graphs).

## ˼·
����һ�����͵�����������⡣ԭ���������ģ���һ������ͼ�У�ÿ���ҵ�һ��û��ǰ
���ڵ�Ľڵ㣨Ҳ�������Ϊ0�Ľڵ㣩,
Ȼ�����ָ�������ڵ�ı߶�ȥ�����ظ�������̣�ֱ�����нڵ㶼�ѱ��ҵ�������û��
���������Ľڵ㣨ͼ���л�����ʱ���������������Ľ⣩��

## Java ����
``` java
public class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        if (numCourses <= 0 || prerequisites == null || prerequisites.length == 0)
            return true;
        Set<Integer> noPreSet = new HashSet<Integer>();
        Map<Integer, List<Integer>> preMap = new HashMap<Integer, List<Integer>>();
        int [] dependCount = new int [numCourses];
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
                return false;
        }

        return true;
    }
}

```
