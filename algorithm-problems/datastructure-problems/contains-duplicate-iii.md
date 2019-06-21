# Contains Duplicate III

_update Sep 9, 2017 14:08_

[LeetCode](https://leetcode.com/problems/contains-duplicate-iii/description/)

Given an array of integers, find out whether there are two distinct indices i and j in the array such that the absolute difference between `nums[i]` and `nums[j]` is at most t and the absolute difference between i and j is at most k.

## Basic Idea:

这道题其实是问在给定数组nums中，是否有一个window（size为k+1）使得window内有两个元素的绝对值差不大于 t；

**思路 1：TreeSet** 首先想到的思路是用 sliding window 的思路，每移动一次窗口，就扫描一遍窗口中的 k 个元素，看能否和刚进入的最右边元素满足要求，时间复杂度为`O(n * min(k,n))`。但是这么做显然太慢了。接下来就想到可以优化每次移动窗口后的比较过程，利用一个 TreeSet，这样每次的比较时间复杂度就从 O\(k\) 变成了 O\(logk\), 总的时间也变为了 `O(n * min(logn, logk))`;

Java Code:

```java
    class Solution {
        public boolean containsNearbyAlmostDuplicate(int[] nums, int k, int t) {
            if (nums == null || nums.length < 2) return false;
            TreeSet<Integer> treeset = new TreeSet<>();
            int i = 0, j = 1;
            treeset.add(nums[0]);
            while (j < nums.length) {
                if (j - i > k) {
                    treeset.remove(nums[i]); // 窗口 left 右移，删去出去的num
                    i++;
                }
                if (treeset.ceiling(nums[j]) != null && Math.abs((long)treeset.ceiling(nums[j]) - (long)nums[j]) <= t)
                    return true;  //  check successor
                if (treeset.floor(nums[j]) != null && Math.abs((long)treeset.floor(nums[j]) - (long)nums[j]) <= t)
                    return true;  //  check predecessor
                treeset.add(nums[j]);
                j++;
            }
            return false;
        }
    }
```

**思路 2：Bucket** 每个 bucket 的size为 t，维持这个buckets，每次右边刚进入窗口的数 m，我们检查 m 所在bucket中是否有元素，若没有，则检查 m 所在bucket的左边和右边相邻有无元素即可。  
时间： O\(n\)

Python Code:

```python
    class Solution(object):
        def containsNearbyAlmostDuplicate(self, nums, k, t):
            """
            :type nums: List[int]
            :type k: int
            :type t: int
            :rtype: bool
            """
            def getIndex(num):
                if t == 0: return num - t
                return (num - t) // t

            if not nums or len(nums) < 2 or k < 1 or t < 0:
                return False

            buckets = {} # Map<index, num> 因为每个bucket只能有一个数字（出现两个就返回true了）
            buckets[getIndex(nums[0])] = nums[0]
            i, j = 0, 1
            while j < len(nums):
                if j - i > k:
                    del buckets[getIndex(nums[i])]
                    i += 1
                index = getIndex(nums[j])
                if index in buckets:
                    return True
                if index - 1 in buckets and abs(buckets[index - 1] - nums[j]) <= t:
                    return True
                if index + 1 in buckets and abs(buckets[index + 1] - nums[j]) <= t:
                    return True
                buckets[index] = nums[j]
                j += 1
            return False
```

_update 2018-07-21 17:27:05_

## Update Bucket sort solution

思路如下：

1. 先将input array normalize 为 最小值为 0 的数组；
2. 用一个hashMap来存放buckets，`bucket index = num / (t + 1)`；
3. 保持 window size 为 `k + 1`，每次查看当前数字对应 bucket index 是否已有元素，若有，则该元素与当前数字的绝对值差一定小于等于 t，直接返回true；否则的话，查看 index - 1 和 index + 1 两个buckets，看其中的元素和当前数字的距离是否小于等于 t；

```java
class Solution {
    public boolean containsNearbyAlmostDuplicate(int[] nums, int k, int t) {
        if (t < 0) return false;
        // normalize first
        long[] longNums = new long[nums.length];
        long min = Integer.MAX_VALUE;
        for (int num : nums) {
            min = Math.min(min, num);
        }
        for (int i = 0; i < nums.length; ++i) longNums[i] = (long)nums[i] - min;
        Map<Long, Long> buckets = new HashMap<>();
        int left = 0, right = -1;
        while (true) {
            while (right - left + 1 < k + 1) {
                if (right == nums.length - 1) return false;
                long num = longNums[++right];
                long index = num / (t + 1);
                if (buckets.containsKey(index)) return true;
                Long prev = buckets.get(index - 1);
                Long next = buckets.get(index + 1);
                if (prev != null && Math.abs(num - prev) <= t) return true;
                if (next != null && Math.abs(num - next) <= t) return true;
                buckets.put(index, num);
            }
            buckets.remove(longNums[left++] / (t + 1));
        }
    }
}
```

