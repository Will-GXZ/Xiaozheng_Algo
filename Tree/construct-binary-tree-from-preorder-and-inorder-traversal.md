## Construct Binary Tree from Preorder and Inorder Traversal

_update Aug 29, 2017  23:07_

---

[Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/description//)



Given preorder and inorder traversal of a tree, construct the binary tree.

**Note:**  
You may assume that duplicates do not exist in the tree.



#### Basic Idea:

通过观察，我们可以发现 preorder 遍历的结果的第一个数一定是 root，而在 inorder list 中，root 会把左右两子树一分为二，利用这种性质，我们可以得到一种递归解法：

* 对于输入inorder lst 和 preorder lst，先标记 preorde\[0\] 作为 root，然后在 inorder 中查找 root 的位置；
* 这样就将两个 list 分为了两部分，分表为当前root左右子树的元素，对左右子树递归执行；

例如，对于input：

```
preorder: [1,2,4,5,7,3,6]
inorder:  [4,2,7,5,1,3,6]
```

我们可以看到，对于以 1 为 root 的树，我们可以用 1 将 inorder 分成两部分，递归传给下层函数的参数分别为：

```
left:                        right:
    preorder: [2,3,5,7]            preorder: [3,6]
    inorder   [4,2,7,5]            inorder:  [3,6]
```

#### Java Code:

```java
    class Solution {
        public TreeNode buildTree(int[] preorder, int[] inorder) {
            return helper(preorder, inorder);
        }
        private TreeNode helper(int[] preorder, int[] inorder) {
            if (preorder.length == 0 ) return null;
            if (preorder.length == 1) return new TreeNode(preorder[0]);
            TreeNode root = new TreeNode(preorder[0]);
            int root_idx = indexOf(inorder, preorder[0]);
            int l_subtree_size = root_idx;
            int r_subtree_size = inorder.length - root_idx - 1;
            if (l_subtree_size > 0) {
                root.left = helper(Arrays.copyOfRange(preorder, 1, l_subtree_size + 1), Arrays.copyOfRange(inorder, 0, root_idx));
            }
            if (r_subtree_size > 0) {
                root.right = helper(Arrays.copyOfRange(preorder, l_subtree_size + 1, preorder.length), 
                                    Arrays.copyOfRange(inorder, root_idx + 1, inorder.length));
            }
            return root;
        }
        private int indexOf(int[] arr, int target) {
            for (int i = 0; i < arr.length; ++i) {
                if (arr[i] == target) return i;
            }
            return -1;
        }
    }
```

#### Python Code:

```python
    class Solution(object):
        def buildTree(self, preorder, inorder):
            """
            :type preorder: List[int]
            :type inorder: List[int]
            :rtype: TreeNode
            """
            if not preorder:
                return None
            if len(preorder) == 1:
                return TreeNode(preorder[0])
            root = TreeNode(preorder[0])
            root_idx = inorder.index(preorder[0])
            left_size = root_idx
            right_size = len(inorder) - root_idx - 1
            if left_size:
                root.left = self.buildTree(preorder[1 : left_size + 1], inorder[0 : root_idx])
            if right_size:
                root.right = self.buildTree(preorder[left_size + 1 :], inorder[root_idx + 1 :])
            return root
```



