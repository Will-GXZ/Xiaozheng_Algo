# Maximum Average Subarray

_update Aug 17, 2017 18:51_

[LeetCode](https://leetcode.com/problems/maximum-average-subarray-i/description/)

Given an array consisting of n integers, find the contiguous subarray of given length k that has the maximum average value. And you need to output the maximum average value.

**Example 1:**

```text
Input: [1,12,-5,-6,50,3], k = 4
Output: 12.75
```

Explanation: Maximum average is \(12-5-6+50\)/4 = 51/4 = 12.75

**Note:** 1 &lt;= k &lt;= n &lt;= 30,000. Elements of the given array will be in the range \[-10,000, 10,000\].

## Basic Idea:

比较好的有两种思路，前缀和 以及 sliding window。时间复杂度都是O\(n\)。这里贴一个前缀和的java Code。

此题的 follow up 就是 [Maximum Average Subarray II](https://will-gxz.gitbooks.io/xiaozheng_algo/content/binary-search/maximum-average-subarray-ii.html)，条件变成了长度不小于 k 的连续子序列的最大平均值。

Java Code:

```java
    class Solution {
        public double findMaxAverage(int[] nums, int k) {
            int maxSum = Integer.MIN_VALUE;
            int[] sums = new int[nums.length + 1];
            for (int i = 1; i < sums.length; ++i) {
                sums[i] = sums[i - 1] + nums[i - 1];
                if (i >= k) maxSum = Math.max(maxSum, sums[i] - sums[i - k]);
            }
            return maxSum / (k * 1.0);
        }
    }
```

_update Nov 30, 2017_

## 更新

**更新一个Java 的 sliding window solution：**

```java
    // sliding window algorithm
    class Solution {
        public double findMaxAverage(int[] nums, int k) {
            double maxAvg = (double)Integer.MIN_VALUE;
            double currSum = 0;
            int left = 0, right = -1; // right 初始值为 -1 是为了令 nums[0] 被考虑在内
            while (true) {
                if (right - left + 1 < k) {
                    right++;
                    if (right >= nums.length) break;
                    currSum += nums[right];
                } else { // 此时 window size 一定等于 k
                    maxAvg = Math.max(maxAvg, currSum / k);
                    currSum -= nums[left];
                    left++;
                }
            }
            return maxAvg;
        }
    }
```

注意这里 sliding window 的写法，while 中第一个 if 判断是否需要扩大当前窗口（right++），else 表示当前窗口大小适宜。感觉这种写法逻辑比较清晰，比几个月前自己写的sliding window 要好理解。

**再写一个 preSum 解法的 python：**

```python
    class Solution:
        def findMaxAverage(self, nums, k):
            """
            :type nums: List[int]
            :type k: int
            :rtype: float
            """
            preSum = [0] * (len(nums) + 1)
            maxAvg = float('-inf')
            for i in range(len(nums)):
                preSum[i + 1] = preSum[i] + nums[i]
                if i >= k - 1:
                    maxAvg = max(maxAvg, (preSum[i + 1] - preSum[i + 1 - k]) / k)
            return maxAvg
```

_update 2018-07-07 10:54:54_

## Update: Sliding window 最终框架

这是一直觉得比较好的 sliding window 的框架写法：

用一层while循环通过移动right指针维持window size。

```java
class Solution {
    public double findMaxAverage(int[] nums, int k) {
        int left = 0, right = -1;
        long max = Integer.MIN_VALUE;
        long window = 0;
        while (true) {
            while (right - left + 1 < k) {
                ++right;
                if (right >= nums.length) break;
                window += nums[right];
            }
            if (right >= nums.length) break;
            max = Math.max(max, window);
            window -= nums[left++];
        }
        return (double)max / k;
    }
}
```

