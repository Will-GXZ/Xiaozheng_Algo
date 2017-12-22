## Binary Tree Path Sum III
_update Aug 28, 2017  23:13_

---
[LintCode](http://www.lintcode.com/en/problem/binary-tree-path-sum-iii/)

Give a binary tree, and a target number, find all path that the sum of nodes equal to target, the path could be start and end at any node in the tree.

**Example**
```java    
    
      // Definition of ParentTreeNode:
      class ParentTreeNode {
          public int val;
          public ParentTreeNode parent, left, right;
      }
```
    Given binary tree:
    
          1
         / \
        2   3
       /
      4
    and target = 6. Return :
    
      [
        [2, 4],
        [2, 1, 3],
        [3, 1, 2],
        [4, 2]
      ]
      
#### Basic Idea:
这道题目在LintCode中标记为hard，ladder only，在 leetcode 中不曾出现，难度比较高，但是或许在面试中并不多见；

解题的基本思路是，对于每个node，我们考虑他往左的所有路径和往右的所有路径，然后将两组路径两两配对，检查sum是否等于target；

为了完成这个目的，我们可以写一个helper function, 功能如下：
-  接收一个TreeNode root，返回该node往下的所有路径，例如前面example中的树，输入root，返回[[1],[1,2],[1,2,4],[1,3]];
-  在函数内部，我们得到了左右两子树的所有路径之后，把root.val放在中间，两边的两组路径两两配对，把符合条件的路径加入res；

时间复杂度应该是 O(N^2 * logN)，因为一颗二叉树所有路径个数为 C(N, 2) (因为树种任意两个node之间有且只有一条路径)，我们如上算法中考察了所有路径, 而路径长度为 O(logN)；

#### Python Code:
实现起来的确非常麻烦，如果用Java的话会很复杂；
```python
    class Solution:
        # @param {ParentTreeNode} root the root of binary tree
        # @param {int} target an integer
        # @return {int[][]} all valid paths
        def binaryTreePathSum3(self, root, target):
            def helper(root):
                # ret: list<List<int>>
                ret = []
                if not root:
                    return ret
                left_paths = helper(root.left)
                right_paths = helper(root.right)
                # 分别插入空路径，表示不包含一侧的路径的情况(不拐弯)
                left_paths.append([])
                right_paths.append([])
                
                # 两两配对，符合条件的path加入res
                for left in left_paths:
                    for right in right_paths:
                        # 这里给left的路径反向，因为需要从左到右的顺序，左边的路径应该从下向上
                        currPath = left[::-1] + [root.val] + right[:]
                        if sum(currPath) == target:
                            res.append(currPath)
                del left_paths[-1]
                del right_paths[-1]
                
                # 生成当前node的所有path
                ret = left_paths[:] + right_paths[:] + [[]]
                for i in range(len(ret)):
                    ret[i] = [root.val] + ret[i]
                return ret
                
            res = []
            helper(root)
            # 把得到的所有路径的反向也加入结果
            size = len(res)
            for i in range(size):
                if len(res[i]) > 1:
                    res.append(res[i][::-1])
            return res
```    
<br>

---
_update Dec 22, 2017  0:27_

### Update 
之前写这道题目的时候，没有注意到这是一个 parentTree，每个 node 除了 left 和 right 之外还有一个parent指针。这样就保证了我们可以从树中的**任意一点出发到达另外任意一点**，于是我们可以把这棵树当做一个**无向连通图**，这样题目就变成了**在一个图中找到所有 sum 为 target 的路径**。我们可以用 dfs 从每个 node 出发，搜索所有其他的 node，记录path，当 `path sum == target` 的时候，将 path 的副本加入 res。

这个做法的**时间复杂度为 O(n^2)**:   
&emsp; 因为在二叉树中任意两点间有且只有一条 path，我们的算法从每个点出发找到其他所有点，故总的时间复杂度为 O(n^2);

这种实现方法比之前的不利用parent的要容易了很多，基本上变成了一个简单的图的dfs。

**Java Code:**
```java
    public class Solution {
        /*
         * @param root: the root of binary tree
         * @param target: An integer
         * @return: all valid paths
         */
        public List<List<Integer>> binaryTreePathSum3(ParentTreeNode root, int target) {
            List<List<Integer>> res = new ArrayList<>();
            if (root == null) return res;
            
            // 先traverse，得到所有的node
            List<ParentTreeNode> treeNodeList = new ArrayList<>();
            Deque<ParentTreeNode> stack = new LinkedList<>();
            stack.addLast(root);
            while (! stack.isEmpty()) {
                ParentTreeNode node = stack.removeLast();
                treeNodeList.add(node);
                if (node.left != null) stack.addLast(node.left);
                if (node.right != null) stack.addLast(node.right);
            }
            
            // 对每个node，dfs考虑所有从该node出发的所有路径
            for (ParentTreeNode node : treeNodeList) {
                dfs(node, target, new HashSet<ParentTreeNode>(), 
                    new ArrayList<Integer>(), 0, res);
            }
            
            return res;
        }
        
        // 从一点出发，dfs，检查其到其他所有点的路径和
        private void dfs(ParentTreeNode root, int target, Set<ParentTreeNode> visited, 
                         List<Integer> path, int pathSum, List<List<Integer>> res) {
            if (root == null || ! visited.add(root)) return; // if already visited root, return
            path.add(root.val);
            pathSum += root.val;
            if (pathSum == target) res.add(new ArrayList<>(path));
            // keep search left, right, parent
            dfs(root.left, target, visited, path, pathSum, res);
            dfs(root.right, target, visited, path, pathSum, res);
            dfs(root.parent, target, visited, path, pathSum, res);
            path.remove(path.size() - 1);
        }
    }
```














        
      
      