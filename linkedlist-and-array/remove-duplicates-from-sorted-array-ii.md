## Remove Duplicates from Sorted Array II
_update Sep 2, 2017  21:48_

---
[LeetCode](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/description/)

Follow up for "Remove Duplicates":
What if duplicates are allowed at most twice?

**For example,**
Given sorted array nums = [1,1,1,2,2,3],

Your function should return length = 5, with the first five elements of nums being 1, 1, 2, 2 and 3. It doesn't matter what you leave beyond the new length.

#### Basic Idea:
这是上一道题目的Follow up，不同于去除重复，这道题要求去除出现次数大于 2 的数，并且同样要 inplace， O(n) time；

我们采用和上一道题类似的 2 pointers 的 partition 方法，维持两个指针 i, j, 以及一个整形变量 dup 来记录当前已经重复的数字：
-  i 指向当前符合要求的数组的最后一位；
-  j 指向当前仍需验证的数组的第一位；
-  每次 j 向后扫描，如果nums[j]==nums[i]，且 dup != nums[j], 就让 dup=nums[j]，表示已经重复了一次，下次如果再相等，并且 dup==nums[j]，则表示当前 j 所指向的数字需要移除，则直接 j++；

#### Python Code:
```python
    # 第一次重复，把dup设为重复的数，若再次重复，则开始删除步骤（2 pointers）
    class Solution(object):
        def removeDuplicates(self, nums):
            """
            :type nums: List[int]
            :rtype: int
            """
            if not nums:
                return 0
            dup = None
            i, j = 0, 1
            while j < len(nums):
                if nums[i] != nums[j]:
                    # 不相等
                    i += 1
                    nums[i] = nums[j]
                    j += 1
                elif nums[i] == nums[j] and dup != nums[i]:
                    # 第一次相等
                    dup = nums[i]
                    i += 1
                    nums[i] = nums[j]
                    j += 1
                else:
                    # 超过一次相等
                    j += 1
            return i + 1
```

---
_update Nov 27, 2017  11:32_

#### 思路更新
不同于之前的思路（记录重复数字本身），我们可以转而直接考虑重复次数（相同数字的个数），因为原数组是排序的，我们只要在出现重复之后令 count++。需要注意的是 count 的初始值应是 1。

```java
    class Solution {
        public int removeDuplicates(int[] nums) {
            if (nums.length == 0) return 0;
            int count = 1;
            int left = 0, right = 1;
            while (right < nums.length) {
                // 如果right指向数字和当前数字不同, 则直接把它换到left+1，并且把count 置 1
                if (nums[left] != nums[right]) {
                    nums[++left] = nums[right++];
                    count = 1;
                } else if (nums[left] == nums[right] && count < 2) {
                    // 如果left指向数字和right指向数字相同，且count < 2， 则换到left+1，并且count++
                    nums[++left] = nums[right++];
                    count++;
                } else {
                    // 否则只把right++
                    right++;
                }
            }
            return left + 1;
        }
    }
```




