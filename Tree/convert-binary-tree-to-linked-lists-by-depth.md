## Convert Binary Tree to Linked Lists by Depth 
_update Sep 14,2017  20:47_

---
[LeetCode](http://www.lintcode.com/en/problem/convert-binary-tree-to-linked-lists-by-depth/)

Given a binary tree, design an algorithm which creates a linked list of all the nodes at each depth (e.g., if you have a tree with depth D, you'll have D linked lists).


**Example**

      Given binary tree:
    
        1
       / \
      2   3
     /
    4
    return
    
    [
      1->null,
      2->3->null,
      4->null
    ]

#### Basic Idea:
基本框架是一个BFS分层遍历，每层在开始 poll 之前都要定义好一个 dummy list node 作为head节点，然后开始本层遍历，每个node的值加到 dummy 后面，每层结束后，该层的 head 就是 dummy.next;

#### Python Code：
```python
    class Solution:
        # @param {TreeNode} root the root of binary tree
        # @return {ListNode[]} a lists of linked list
        def binaryTreeToLists(self, root):
            ret = []
            if not root:
                return ret
            queue = collections.deque()
            queue.appendleft(root)
            while queue:
                size = len(queue)
                dummy = ListNode(0)
                curr = dummy
                for i in range(size):
                    tnode = queue.pop()
                    curr.next = ListNode(tnode.val)
                    curr = curr.next
                    if tnode.left:
                        queue.appendleft(tnode.left)
                    if tnode.right:
                        queue.appendleft(tnode.right)
                ret.append(dummy.next)
            return ret
```
<br>

---
_update Dec 29, 2017 1:09_

#### Update
更新一个 Java solution，和之前一样的方法，分层BFS；
```java
public class Solution {
    /**
     * @param root the root of binary tree
     * @return a lists of linked list
     */
    public List<ListNode> binaryTreeToLists(TreeNode root) {
        List<ListNode> res = new ArrayList<>();
        if (root == null) return res;
        Deque<TreeNode> queue = new LinkedList<>();
        queue.addFirst(root);
        while (! queue.isEmpty()) {
            int size = queue.size();
            ListNode dummy = new ListNode(0);
            ListNode prev = dummy;
            for (int i = 0; i < size; ++i) {
                TreeNode node = queue.removeLast();
                prev.next = new ListNode(node.val);
                prev = prev.next;
                if (node.left != null) queue.addFirst(node.left);
                if (node.right != null) queue.addFirst(node.right);
            }
            res.add(dummy.next);
        }
        return res;
    }
}
```