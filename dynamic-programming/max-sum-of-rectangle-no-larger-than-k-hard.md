# Max Sum of Rectangle No Larger Than K
_update Mar 15, 2018 23:03_

---
[LeetCode](https://leetcode.com/problems/max-sum-of-rectangle-no-larger-than-k/description/)

Given a non-empty 2D matrix matrix and an integer k, find the max sum of a rectangle in the matrix such that its sum is no larger than k.

**Example:**   

    Given matrix = [
      [1,  0, 1],
      [0, -2, 3]
    ]
    k = 2
    
The answer is 2. Because the sum of rectangle `[[0, 1], [-2, 3]]` is 2 and 2 is the max number no larger than `k (k = 2)`.

**Note:**

1. The rectangle inside the matrix must have an area `> 0`.
2. What if the number of rows is much larger than the number of columns?

<br>

## Basic Idea:
这道题的难度主要在于整合了两道题在一起。首先，要知道如何在一个一维数组中求 subarray 的不超过 k 的最大和，进而将这种解法推广到二维情况。

对于这道题，我们可以 `O(N^2)` 时间枚举所有的左右边界对，而对于已经确定左右边界的中间部分，我们可以通过预处理生成每个row的 refix sum 数组，实现 `O(1)` 时间求左右边界之间某一row的和，于是此时这个问题就变成了求一个一维数组的最大不超过 k 的 subarray 的和，总的时间复杂度也就是这一步的时间乘以之前枚举左右边界的次数 `O(N^2)`。

下面着重说一下一维数组求最大不超过 k 的 subarray sum 的解法。解法有两种，都要利用 prefix sum 的技巧：
> **1. 利用 TreeMap（BST)：**   
先求出 prefix sum 数组，然后逐个将 prefix sum 数组中的元素 `sums[i]` 加入 BST，每次加入之前检查BST中之前的使得 `sums[i] - prev < k` 的最小 prev，并用 `sums[i] - prev` 和全局最大值对比，更新全局最大值。每次操作 BST 耗时 `O(logN)`，共需要操作 N 次，所以时间复杂度 `O(NlogN)`;
> **2. 利用排序，双指针：**  
先求出 prefix sum 数组 sums，然后将 sums 排序，利用双指针法从左右两端点向内逼近，第一个小于等于 k 的差值就是最终解，时间复杂度为 `O(NlogN + N) = O(NlogN)`;