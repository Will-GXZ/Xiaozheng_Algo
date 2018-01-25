## Find Median from Data Stream （hard，google）
_update Jan 24,2018  21:02_

---
[LeetCode](https://leetcode.com/problems/find-median-from-data-stream/description/)

Median is the middle value in an ordered integer list. If the size of the list is even, there is no middle value. So the median is the mean of the two middle value.

**Examples: **
    
    [2,3,4] , the median is 3
    [2,3], the median is (2 + 3) / 2 = 2.5

Design a data structure that supports the following two operations:

void addNum(int num) - Add a integer number from the data stream to the data structure.
double findMedian() - Return the median of all elements so far.

**For example:**
    
    addNum(1)
    addNum(2)
    findMedian() -> 1.5
    addNum(3) 
    findMedian() -> 2
    
<br>

### Basic Idea:
拿到这道题目我的思路如下：

  1.  首先想到维持一个Doubly Linked List，这样每次插入新的数字之后，如果比当前 mid 大，mid 就可以右移，否则可以左移。但是这种方法没法高效地找到每次要插入数字的准确位置；
  2.  沿着之前的思路，寻求优化，于是想到我们要做的事情的本质其实是将数据分成两段大小相等的部分，并且维持前半段的最大值max，和后半段的最小值min，每次求中位数的时候，一定来自这两个 min 和 max；
  3.  于是，自然想到维持两个大小为 `n/2` 的 heap，前半段用 maxHeap，后半段用 minHeap















