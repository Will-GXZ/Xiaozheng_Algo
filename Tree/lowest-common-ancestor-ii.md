## Lowest Common Ancestor II
_update Aug 27, 2017  22:06_

---
[LintCode](http://www.lintcode.com/en/problem/lowest-common-ancestor-ii/)

Given the root and two nodes in a Binary Tree. Find the lowest common ancestor(LCA) of the two nodes.

The lowest common ancestor is the node with largest depth which is the ancestor of both nodes.

The node has an extra attribute parent which point to the father of itself. The root's parent is null.

**Example**

      For the following binary tree:
      
        4
       / \
      3   7
         / \
        5   6
      LCA(3, 5) = 4
      
      LCA(5, 6) = 7
      
      LCA(6, 7) = 7

#### Basic Idea:
由于这道题的二叉树给了父节点，我们可以从A，B开始往上直到root，分别得到两个链表，如果A，B的公共节点存在，那么就相当于两个链表有共同部分（链表分叉）。此时我们只要从root开始向下，比较每个同样位置的元素，直到下一个开始分叉位置。

#### Java Code:
```java
    public class Solution {
        /**
         * @param root: The root of the tree
         * @param A, B: Two node in the tree
         * @return: The lowest common ancestor of A and B
         */
        public ParentTreeNode lowestCommonAncestorII(ParentTreeNode root,
                                                     ParentTreeNode A,
                                                     ParentTreeNode B) {
            List<ParentTreeNode> lstA = new ArrayList<>();
            List<ParentTreeNode> lstB = new ArrayList<>();
            ParentTreeNode temp = A;
            while (temp != null) {
                lstA.add(temp);
                temp = temp.parent;
            }
            temp = B;
            while (temp != null) {
                lstB.add(temp);
                temp = temp.parent;
            }
            int len = Math.min(lstA.size(), lstB.size());
            for (int i = 0; i < len; ++i) {
                if (lstA.get(lstA.size() - 1) == lstB.get(lstB.size() - 1)) {
                    temp = lstA.remove(lstA.size() - 1);
                    lstB.remove(lstB.size() - 1);
                }    
            }
            return temp;
        }
    }
```
      