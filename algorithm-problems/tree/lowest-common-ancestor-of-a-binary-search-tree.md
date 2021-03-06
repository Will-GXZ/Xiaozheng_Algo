# Lowest Common Ancestor of a Binary Search Tree

_update Jun 27, 2017_

[leetcode](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/#/description)

Given a binary search tree \(BST\), find the lowest common ancestor \(LCA\) of two given nodes in the BST.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes v and w as the lowest node in T that has both v and w as descendants \(where we allow a node to be a descendant of itself\).”

```text
              _______6______
             /              \
          ___2__          ___8__
         /      \        /      \
         0      _4       7       9
               /  \
               3   5
```

For example, the lowest common ancestor \(LCA\) of nodes 2 and 8 is 6. Another example is LCA of nodes 2 and 4 is 2, since a node can be a descendant of itself according to the LCA definition.

## 思路

基本的想法是要利用到其中BST的性质，即最近的祖先的值一定是在两个target nodes之间。

Java Code：

```java
    // java
    public class Solution {
        TreeNode ret = null;
        public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
            if (root == null) return null;
            if (root.val < p.val && root.val < q.val)
                return lowestCommonAncestor(root.right, p, q);
            if (root.val > p.val && root.val > q.val)
                return lowestCommonAncestor(root.left, p, q);
            else
                return root;
        }  
    }
```

_update 2018-06-29 16:08:42_

## Iterative Java Solution

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (p.val > q.val) {
            TreeNode t = p;
            p = q;
            q = t;
        }
        TreeNode node = root;
        while (! (p.val <= node.val && q.val >= node.val)) {
            if (node.val < p.val) node = node.right;
            else node = node.left;
        }
        return node;
    }
}
```

_update Jul 3 2021_

更新一个 Iterative 做法

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        int lower = Math.min(p.val, q.val);
        int upper = Math.max(p.val, q.val);
        while (true) {
            if (root.val < lower) root = root.right;
            else if (root.val > upper) root = root.left;
            else return root;
        }
    }
}
```

