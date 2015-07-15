# Progress 写作进度
- [x] 104 Maximum Depth of Binary Tree
- [x] 111 Minimum Depth of Binary Tree
- [x] 174 Dungeon Game
- [ ] 84  Largest Rectangle in Histogram


# Requirement(编辑要求)
*  Please put the file in the corresponding directory explained in the [file and
   directory arrangement] section. (请按照[目录安排]中的说明书写和放置文件) 
* Please record the problems whose solutions you've written or you are ready to
  write in [Progress] section (请将自己写完和准备写的问题在[写作进度]中注明)
* Please record the problems you've added in
  "problems.md"。(请将自己写完的问题在problem.md中记录)
*  Please comment the problems you've added in the commit message when you
   commit. (每次提交时，请在提交信息中注明增加了哪些问题的解答)
* Please write each solution page following the requirement demonstrated in the
  next section(last item) (请按照下节中的示例来书解答页面)
* Please do not submit the interediate files such as *.html, *~,
    *.swp(请不要将中间文件或临时备份文件提交)
    
  write a file named ".gitignore" in your root directory, containing the
  patterns of files you are ready to ignore. The following content is an example:
  ```
  *.html
  *~
  *.swp
  ``` 
 
# File and Directory Arrangement(目录安排)
* README.md corresponds the preface of the book.
* SUMMARY.md describes the contents of the book and the location of the individual
  chapters.

  For example, 
  <pre>
    # Summary
    * [Part I](part1/README.md)
        * [Writing is nice](part1/writing.md)
        * [GitBook is nice](part1/gitbook.md)
    * [Part II](part2/README.md)
        * [We love feedback](part2/feedback_please.md)
        * [Better tools for authors](part2/better_tools.md)
  </pre>

   describes that there are two chapters named "Part I" and "Part II", which
   each contain two subchapters . The contents of "Part I" and "Part II"
   are located in file "part1/README.md" and "part2/README.md" respectively.

   For the more information, please visit [GitBook Help](http://help.gitbook.com/format/chapters.html) 

* It is planned to write four chapters for now:
    * [Problems](problems.md)

    Record all the problems explained in this book, and the links to the
    solutions in this book.

    All Problems are intended to be sorted by their indexed in leetcode finally.
    But the order can be ignored temporarily.

    * [Recursion](recursion/README.md)

    Record all the solutions of the problems which can be solved by recursion.
    All the solutions are located in directory "recursion".

    The problems are classfied by data structures, i.e. tree, list, string etc.,
    and the solutions which belong to the same category are put into the same
    directory. For example, all solutions of the problems related to tree are
    located in directory "recursion/tree". If a problem cannot be categorized
    apparently, it is thrown in to the "recursion/other" directory.

    * Record all the solutions of the problems which can be solved by dynamic
      programming.

      The classfication method is not determined now, just put them into the
      same directory named "dp".

    * [Other](other/README.md)

    All the problems cannot be solved  by recursion and dynamic programming are
    collected here. The directory is "other".
* Solution Page Example: 
<pre>
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
    public class Solution {
        public int maxDepth(TreeNode root) {
           if (root == null)
               return 0;
            return (Math.max(maxDepth(root.left),maxDepth(root.right)) + 1);
        }
    }
</pre>

# Editing Way(编辑方式)
* Edit on Github

clone this repository, edit and submit, the gitbook will be updated
automatically.

This way is recommended, because gitbook.com is loading very slow.

* Edit on Gitbook

On www.gitbook.com, it allows online editing and the preview of the markdown
file is shown simultaneously when you edit.

* Generate git book offline

[Gitbook简明教程](http://www.colobu.com/2014/10/09/gitbook-quickstart/)


# 前言
刷LeetCode是一段艰辛的旅程，特别对于像我这样算法不是很扎实，平时脱离编程的人。
但为了找到一份像样的工作，还是硬着头皮刷了。编程、计算机从来也没有成为我的爱，
虽然和它打交道了十年，最终它还是只沦为了混饭吃的工具。

正是由于自己在算法方面悟性和经验的欠缺，在刷题时，参考了很多别人的代码。为了
不让自己那么快就忘掉，决定把每道题的思路都记录下来。好记心不如烂笔头，脑袋里
想不清楚，那就在就在纸上写写画画，未必能让你通透地理解，至少对你的记忆也是有
帮助的。

之前，也有一些人将LeetCode的题解([siddontang][1],[soulmachine][2],[yuanbin][3])
整理发布了，但他们大多将题目按照数据
结构来分类，而不是按照思路和解法来分类。这种分类方式对于更好地回顾，让
知识更成体系有帮助，但对于引导思路来讲，还是显得零碎了一些。我希望尽量将题目按
照解题的思路来分类，从而找到其中的门路，遇到了新的题，至少就不会那么茫然无措了。

算法的基本思路主要有枚举、递归、贪心、动态规划。其中:
* 动态规划能解的题通常有递归解法。
* 贪心算法可以看做将问题分解为1和n-1子问题的递归解法。
* 递归算法又可以分为：
    * 分治法：将n问题分解为两个n/2子问题 
    * 其它分法：如将n问题分解为1和n-1子问题，或者分解为n-1和1子问题，或者分为3段
      等等。
* 枚举重要的是不能将一个可能的解重复验证。枚举的方式因题目的不同而不同，不过对于
    常用的数据结构，还是有一些经典的枚举算法。如果能把题目转化为这些数据结构上的
    问题，就能够更容易地找到枚举方法。
    * 图：
        * 深度优先搜索算法
        * 广度优先搜索算法
    * 树：
        * 先序遍历
        * 中序遍历
        * 后序遍历

除了上述的几种通用思路外，还有一些想法，对你解题也会有一些帮助：
* 看有没有长得像的经典题目和算法。当然，这就要求你的脑袋里有一些存货。
* 剪枝。只要你多花点时间，一个问题的枚举解法，你总能找到。如果效率不是很高的话，
    那就试着想想哪些值是不可能成为解的。然后在枚举的过程中，直接将那些不可能的
    值剔除。





[1]: https://www.gitbook.com/book/siddontang/leetcode-solution/details
[2]: https://github.com/soulmachine/leetcode 
[3]: http://algorithm.yuanbin.me/
