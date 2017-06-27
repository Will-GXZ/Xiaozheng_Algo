## [437. Path Sum III](https://leetcode.com/problems/path-sum-iii/#/solutions)
_update Jun 27, 2017_

---
You are given a binary tree in which each node contains an integer value.

Find the number of paths that sum to a given value.

The path does not need to start or end at the root or a leaf, but it must go downwards (traveling only from parent nodes to child nodes).

The tree has no more than 1,000 nodes and the values are in the range -1,000,000 to 1,000,000.

**Example**:

root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8

            10
           /  \
          5   -3
         / \    \
        3   2   11
       / \   \
      3  -2   1

Return 3. The paths that sum to 8 are:

1.  5 -> 3
2.  5 -> 2 -> 1
3. -3 -> 11

**解释**下：这道题的意思就是说找任意path上相邻节点的和是 given sum value 的数量。
**思路**： 这道题目的难点是要考虑从上到下每一段path上nodes的和。我们可以写两个 recursive function， 外层的用来对每个node（做为root）考虑其子树中的所有path。而内层 function 则用来考虑包含该 node（做为root）的所有path。
**细节**： 对于内层function，没次调用下一层时候，传入下层的 sum 变为 sum - node.val，这样当下层 val == sum 的时候，就说明有了一条满足条件的path。

Python Code：
```python
    # python 2.7
    class Solution(object):
        def pathSum(self, root, sum):
            """
            :type root: TreeNode
            :type sum: int
            :rtype: int
            """
            if not root:
                return 0
            return self.dfs(root, sum) + self.pathSum(root.left, sum) + self.pathSum(root.right, sum)
            
        def dfs(self, node, target):
            if not node: 
                return 0
            return (node.val == target) + self.dfs(node.left, target - node.val) + self.dfs(node.right, target - node.val)
```