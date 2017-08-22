## Merge k Sorted Lists
_update Aug 22,2017  15:56_

---
[LintCode](http://www.lintcode.com/en/problem/merge-k-sorted-lists/)
[LeetCode](https://leetcode.com/problems/merge-k-sorted-lists/description/)

Merge k sorted linked lists and return it as one sorted list.

Analyze and describe its complexity.

**Example**

      Given lists:
      
      [
         2->4->null,
         null,
         -1->null
      ],
      return -1->2->4->null.
      
#### Basic Idea:
利用 min heap，先把每个 list 的 head 加入 heap，每次拿出当前 heap 的 peek 作为当前最小值，放入 res，再把该最小node 的 next 加入heap。这样一来，就可以保证当前 heap 的顶一定为剩余所有node中最小的。

时间复杂度为O(nklogk), 因为我们每个node都访问了一遍（nk），对于每个node，需要logk的heap操作，所以是 nklogk。空间复杂度为 O(k)，即 priority queue 的 size；

#### Java Code：
    ```java
    /**
     * Definition for ListNode.
     * public class ListNode {
     *     int val;
     *     ListNode next;
     *     ListNode(int val) {
     *         this.val = val;
     *         this.next = null;
     *     }
     * }
     */ 
    public class Solution {
        /**
         * @param lists: a list of ListNode
         * @return: The head of one sorted list.
         */
        public ListNode mergeKLists(List<ListNode> lists) {  
            // using a min heap
            if (lists == null || lists.size() == 0) return null;
            PriorityQueue<ListNode> pq = new PriorityQueue<>(lists.size(), new Comparator<ListNode>(){
                @Override
                public int compare(ListNode node1, ListNode node2) {
                    return node1.val - node2.val;
                }
            });
            for (ListNode node : lists) {
                if (node != null) {
                    pq.offer(node);
                }
            }
            ListNode dummy = new ListNode(0);
            ListNode curr = dummy;
            while (! pq.isEmpty()) {
                ListNode currMin = pq.poll();
                if (currMin.next != null) {
                    pq.offer(currMin.next);
                }
                curr.next = new ListNode(currMin.val);
                curr = curr.next;
            }
            return dummy.next;
        }
    }
```