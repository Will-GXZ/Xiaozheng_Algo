# Check Completeness of a Binary Tree
_update Dec 25, 16:19_

---
[LeetCode](https://leetcode.com/problems/check-completeness-of-a-binary-tree/)

Given a binary tree, determine if it is a complete binary tree.

Definition of a complete binary tree from Wikipedia:
In a complete binary tree every level, except possibly the last, is completely filled, and all nodes in the last level are as far left as possible. It can have between 1 and 2h nodes inclusive at the last level h.

 

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/12/15/complete-binary-tree-1.png)

    Input: [1,2,3,4,5,6]
    Output: true
    Explanation: Every level before the last is full (ie. levels
      with node-values {1} and {2, 3}), and all nodes in the last
      level ({4, 5, 6}) are as far left as possible.

**Example 2:**

![](https://assets.leetcode.com/uploads/2018/12/15/complete-binary-tree-2.png)

    Input: [1,2,3,4,5,null,7]
    Output: false
    Explanation: The node with value 7 isn't as far left as possible.
 
**Note:**

The tree will have between 1 and 100 nodes.

<br/>

### Basic Idea:
我们注意到，如果一个树是完全二叉树，那么在BFS的过程中，一旦我们遇到null，后面就不可以有不是null的node，我们可以利用这一性质。简单来说就是写一个BFS，用一个boolean标记是否已经见过null，如果见过，当我们再遇到非null时直接返回false；

#### Java Code:
```java
class Solution {
    public boolean isCompleteTree(TreeNode root) {
        Deque<TreeNode> queue = new LinkedList<>();
        queue.offerLast(root);
        boolean seenNull = false;
        while (! queue.isEmpty()) {
            TreeNode currNode = queue.pollFirst();
            if (currNode == null) {
                seenNull = true;
                continue;
            } else if (seenNull) {
                return false;
            } 
            queue.offerLast(currNode.left);
            queue.offerLast(currNode.right);
        }
        return true;
    }
}
```
