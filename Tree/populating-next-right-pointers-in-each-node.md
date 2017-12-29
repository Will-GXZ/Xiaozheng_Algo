## Populating Next Right Pointers in Each Node
_update Sep 1, 2017  23:10_

---
[LeetCode](https://leetcode.com/problems/populating-next-right-pointers-in-each-node/description/)

Given a binary tree
```c
    struct TreeLinkNode {
      TreeLinkNode *left;
      TreeLinkNode *right;
      TreeLinkNode *next;
    }
```
Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.

Initially, all next pointers are set to NULL.

**Note:**

You may only use constant extra space.
You may assume that it is a perfect binary tree (ie, all leaves are at the same level, and every parent has two children).

**For example,**

    Given the following perfect binary tree,
             1
           /  \
          2    3
         / \  / \
        4  5  6  7
    After calling your function, the tree should look like:
             1 -> NULL
           /  \
          2 -> 3 -> NULL
         / \  / \
        4->5->6->7 -> NULL
        
#### Basic Idea:
[这里](https://siddontang.gitbooks.io/leetcode-solution/content/tree/populating_next_right_pointers_in_each_node.html) 有一个不错的分析。

题目要求 O(1)space, 说明不能用 BFS。注意到我们可以利用构建好的 next 指针在同层中移动，利用这条性质可以避免 extra space；

同时，因为所给树是一个完美满二叉树，我们注意到如下规律：
```java
    node.left.next = node.right
    node.right.next = node.next.left
```

#### Java Code:
```java
public class Solution {
    public void connect(TreeLinkNode root) {
        TreeLinkNode first = root;
        // 用一个first沿着左边边线向下遍历
        while (first != null) {
            // 用curr和一个内层循环，横向遍历每层
            TreeLinkNode curr = first;
            while (curr.left != null) {
                curr.left.next = curr.right;
                if (curr.next == null) break;
                curr.right.next = curr.next.left;
                curr = curr.next;
            }
            first = first.left;
        }
    }
}
```

---
## Populating Next Right Pointers in Each Node II

[LeetCode](https://leetcode.com/problems/populating-next-right-pointers-in-each-node-ii/description/)

Follow up for problem "Populating Next Right Pointers in Each Node".

What if the given tree could be any binary tree? Would your previous solution still work?

**Note:**

You may only use constant extra space.

**For example,**

    Given the following binary tree,
             1
           /  \
          2    3
         / \    \
        4   5    7
    After calling your function, the tree should look like:
             1 -> NULL
           /  \
          2 -> 3 -> NULL
         / \    \
        4-> 5 -> 7 -> NULL)
    
#### Basic Idea:
推广到任意二叉树后，前面的解法就不成立了，需要进行修正。这道题目中，任务变成了已知上层的next关系，推导下层的next关系；

这次，不再无脑使用：
```java
    node.left.next = node.right
    node.right.next = node.next.left
```
我们还需要加入判断。对于first，不再无脑往left走，而是每层循环之前将下一层的first设为null，用本层第一个child更新下一层的first。用变量lastNode跟踪之前一个visit过的node，即当前需要为其设置next的node。

**Java Code:**  
```java
public class Solution {
    public void connect(TreeLinkNode root) {
        TreeLinkNode first = root;
        while (first != null) {
            TreeLinkNode curr = first;
            first = null; // 先将first置空，方便判断下一个first是否被设置
            TreeLinkNode lastNode = null; // 用 lastNode 跟踪当前最近需要被设置next的node
            while (curr != null) {
                TreeLinkNode[] children = new TreeLinkNode[] {curr.left, curr.right};
                for (TreeLinkNode child : children) {
                    if (child != null) {
                        if (first == null) first = child; // 如果下一层的first没设置，则当前child一定是第一个
                        if (lastNode == null) lastNode = child; // 当前child是本层第一个，设为lastNode等待设置其next
                        else {
                            lastNode.next = child;
                            lastNode = child;
                        }
                    }
                }
                curr = curr.next;
            }
        }
    }
}
```




