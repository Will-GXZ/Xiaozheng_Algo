# Find Mode in Binary Search Tree

_update Jun 27, 2017_

[leetcode](https://leetcode.com/problems/find-mode-in-binary-search-tree/#/description)

Given a binary search tree \(BST\) with duplicates, find all the mode\(s\) \(the most frequently occurred element\) in the given BST.

Assume a BST is defined as follows:

The left subtree of a node contains only nodes with keys less than or equal to the node's key. The right subtree of a node contains only nodes with keys greater than or equal to the node's key. Both the left and right subtrees must also be binary search trees. For example: Given BST \[1,null,2,2\]

```text
     1
       \
         2
       /
     2
```

return \[2\].

Note: If a tree has more than one mode, you can return them in any order.

_Follow up_: Could you do that without using any extra space? \(Assume that the implicit stack space incurred due to recursion does not count\).

### 思路

这道题的难点是不可以使用 counting map 暴力统计每个数字的频数，所以我们只有利用BST的性质，用 inorder traverse 把问题化简为一个求 sorted array 中最大频数的 element 的问题。我们可以 maintain 一个 `count`和一个`max`，同时跟踪`prev node`，如果`curr node.val == prev node.val`，我们令`count++`，如果`count == max`或者 `count > max`，我们再做出相应的操作。结果存在一个`list`中。

Java recursive code：

```java
    // java
    public class Solution {
        int max = -1;
        int count = -1;
        int prev = Integer.MAX_VALUE;
        public int[] findMode(TreeNode root) {
            if (root == null) return new int[] {};
            List<Integer> lst = new ArrayList<>();
            inOrder(root, lst);
            int[] ret = new int[lst.size()];
            for (int i = 0; i < ret.length; ++i) {
                ret[i] = lst.get(i);
            }
            return ret;
        }
        private void inOrder(TreeNode node, List<Integer> lst) {
            if (node == null) return;
            inOrder(node.left, lst);
            if (prev == node.val) {
                // curr == prev，则count++
                count++;
            } else {
                count = 1;
                prev = node.val;
            }
            if (count > max) {
            // 说明之前的max太小，所以清空list重新添加最大频率元素
                lst.clear();
                lst.add(node.val);
                max = count;
            } else if (count == max) {
                lst.add(node.val);
            }
            inOrder(node.right, lst);
        }
    }
```

_update Dec 19, 2017 16:42_

## Update

要特别注意，更新 prev 和 更新 res list 是两回事，

**python code**

```python
    class Solution:
        def findMode(self, root):
            """
            :type root: TreeNode
            :rtype: List[int]
            """
            def helper(root):
                if not root:
                    return
                helper(root.left)
                # 更新 prev， count ，currMaxCount
                if root.val == self.prev:
                    self.count += 1
                else:
                    self.count = 1
                    self.prev = root.val
                # 更新 res list
                if self.count > self.currMaxCount:
                    self.res = [root.val]
                    self.currMaxCount = self.count
                elif self.count == self.currMaxCount:
                    self.res.append(root.val)
                helper(root.right)


            self.count = 0
            self.prev = float('INF')
            self.currMaxCount = float('-inf')
            self.res = []
            helper(root)
            return self.res
```

_update May 10,2018 22:19_

### C++ Code:

**Recursive Solution:**

```cpp
class Solution {
    vector<int> res;
    int prev = 0x7fffffff;
    int count = 0;
    int maxCount = 0;

    void helper(TreeNode* root) {
        if (root == nullptr) return;

        helper(root->left);

        if (root->val == prev) {
            ++count;
        } else {
            prev = root->val;
            count = 1;
        }
        if (count == maxCount) {
            res.push_back(root->val);
        } else if (count > maxCount) {
            res.clear();
            res.push_back(root->val);
            maxCount = count;
        }

        helper(root->right);
    }
public:
    vector<int> findMode(TreeNode* root) {
        helper(root);
        return res;
    }
};
```

**Iterative Solution:**

```cpp
class Solution {
public:
    vector<int> findMode(TreeNode* root) {
        deque<TreeNode*> _stack;
        vector<int> res;
        int prev = 0, count = 0, maxCount = 0;
        if (root == nullptr) return res;

        TreeNode* node = root;
        while (node) {
            _stack.push_back(node);
            node = node->left;
        }

        while (! _stack.empty()) {
            node = _stack.back();
            _stack.pop_back();
            if (prev == node->val) {
                count++;
            } else {
                prev = node->val;
                count = 1;
            }
            if (count > maxCount) {
                res.clear();
                res.push_back(node->val);
                maxCount = count;
            } else if (count == maxCount) {
                res.push_back(node->val);
            }
            node = node->right;
            while (node) {
                _stack.push_back(node);
                node = node->left;
            }
        }
        return res;
    }
};
```

