# The Skyline Problem 
## Source
leetcode:[218 The Skyline Problem](https://leetcode.com/problems/the-skyline-problem/)
> A city's skyline is the outer contour of the silhouette formed by all the
> buildings in that city when viewed from a distance. Now suppose you are given
> the locations and height of all the buildings as shown on a cityscape photo
> (Figure A), write a program to output the skyline formed by these buildings
> collectively (Figure B).
> 
> ![skyline1](images/skyline1.jpg)
> ![skyline2](images/skyline2.jpg)
> 
> The geometric information of each building is represented by a triplet of
> integers [Li, Ri, Hi], where Li and Ri are the x coordinates of the left and
> right edge of the ith building, respectively, and Hi is its height. It is
> guaranteed that 0 <= Li, Ri <= INT_MAX, 0 < Hi <= INT_MAX, and Ri - Li > 0. You
> may assume all buildings are perfect rectangles grounded on an absolutely flat
> surface at height 0.
> 
> For instance, the dimensions of all buildings in Figure A are recorded as: [ [2
> 9 10], [3 7 15], [5 12 12], [15 20 10], [19 24 8] ] .
> 
> The output is a list of "key points" (red dots in Figure B) in the format of [
> [x1,y1], [x2, y2], [x3, y3], ... ] that uniquely defines a skyline. A key point
> is the left endpoint of a horizontal line segment. Note that the last key point,
> where the rightmost building ends, is merely used to mark the termination of the
> skyline, and always has zero height. Also, the ground in between any two
> adjacent buildings should be considered part of the skyline contour.
> 
> For instance, the skyline in Figure B should be represented as:[ [2 10], [3 15],
> [7 12], [12 0], [15 10], [20 8], [24, 0] ].
> 
> **Note**:
> * The number of buildings in any input list is guaranteed to be in the range [0,
> 10000].
> * The input list is already sorted in ascending order by the left x position Li.
> * The output list must be sorted by the x position.
> * There must be no consecutive horizontal lines of equal height in the output
> skyline. For instance, [...[2 3], [4 5], [7 5], [11 5], [12 7]...] is not
> acceptable; the three lines of height 5 should be merged into one in the final
> output as such: [...[2 3], [4 5], [12 7], ...]

## ˼·
��������ֱ�ӽⷨ������һ�������������С�����֪n-1��������������Ȼ��������һ��
�����������������ⷨ��ʱ�临�Ӷ�ΪO(n^2)��������㷨��һ���Ż��ǲ��÷��η�������
��ʱ�临�ӶȽ�ΪO(n log n)���������������ؽ����η���

���η��Ļ������Ϊ��������һ�뽨����������Ȼ�����һ�뽨����������Ȼ��������
������Ľ���ϲ����õ����н��������������еĹؼ���������Ľ���ϲ�Ϊԭ����Ľ����

��֪ǰһ�뽨��������Ϊskyline1����һ�뽨���ĵ�����Ϊskyline2��ͨ�����²���õ�
���в����������
* �ҵ�����������x��������С���Ǹ��㣬������Ƿ��ܼ��뵽���Ľ����ȡ����(ע�⣬
    ֻ���µĵ�߶Ⱥ������������һ����ĸ߶Ȳ�ͬʱ���µĵ���ܱ�����)��
    * ������ĸ߶ȱ��������е����һ����ĸ߶ȸߣ��������������
    * ������ĸ߶Ⱥ��������е����һ����ĸ߶���ͬ��������
    * ������ĸ߶ȱ��������е����һ����ĸ߶ȵͣ���ô�����������
        * ���ĸ߶ȱ��������У�����һ�齨�������һ����ĸ߶ȸߣ���ô���������
            ����
        * ���ĸ߶Ȳ����������У�����һ�����������һ����ĸ߶ȸߣ���ô��(x,h)����
        ���������С����У�xΪ��ǰ��ĺ����꣬hΪ������������һ�齨�������һ����
        �ĸ߶ȡ�
* �����齨������ɾ���ոձ������Ľ������ص���һ����



## Java Recursion ����
``` java
public class Solution {
    public List<int[]> getSkyline(int[][] buildings) {
        List<int[]> res = new LinkedList<int[]>();

        if (buildings == null || buildings.length == 0 || buildings[0].length == 0)
            return res;

        return divideConquer(buildings, 0, buildings.length-1);


    }

    public LinkedList<int[]> divideConquer(int [][] buildings, int start, int end) {
        LinkedList<int[]> res = new LinkedList<int[]>();
        if (start == end) {
            int left=buildings[start][0];
            int right=buildings[start][1];
            int h = buildings[start][2];
            res.add(new int[] {left, h});
            res.add(new int[] {right, 0});
            return res;
        } else {
            int len = end-start+1;
            LinkedList<int[]> res1 = divideConquer(buildings, start, start+len/2-1);
            LinkedList<int[]> res2 = divideConquer(buildings, start+len/2, end);
            return merge(res1, res2);
        }


    }

    public LinkedList<int[]> merge(LinkedList<int[]> l1, LinkedList<int[]> l2) {
        LinkedList<int[]> res = new LinkedList<int[]>();
        int h1=0, h2=0;
        int x, h;
        while (l1.size() > 0 && l2.size() > 0) {
            if (l1.getFirst()[0] < l2.getFirst()[0]) {
                h1 = l1.getFirst()[1];
                x = l1.getFirst()[0];
                h = Math.max(h1, h2);
                l1.removeFirst();
            } else if (l1.getFirst()[0] > l2.getFirst()[0]) {
                h2 = l2.getFirst()[1];
                x = l2.getFirst()[0];
                h = Math.max(h2, h1);
                l2.removeFirst();
            } else {
                h1 = l1.getFirst()[1];
                h2 = l2.getFirst()[1];
                x = l1.getFirst()[0];
                h = Math.max(h1, h2);
                l1.removeFirst();
                l2.removeFirst();
            }

            if (res.isEmpty() || h != res.getLast()[1]) {
                res.add(new int[] {x,h});
            }
        }

        res.addAll(l1);
        res.addAll(l2);

        return res;
    }
}

```
