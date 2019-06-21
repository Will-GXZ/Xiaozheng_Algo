# Count Binary Substrings \(Easy\)

_update May 14,2018 17:32_

[LeetCode](https://leetcode.com/problems/count-binary-substrings/description/)

Give a string s, count the number of non-empty \(contiguous\) substrings that have the same number of 0's and 1's, and all the 0's and all the 1's in these substrings are grouped consecutively.

Substrings that occur multiple times are counted the number of times they occur.

**Example 1:**

```text
Input: "00110011"
Output: 6
Explanation: There are 6 substrings that have equal number of consecutive 1's and 0's: 
  "0011", "01", "1100", "10", "0011", and "01".
```

Notice that some of these substrings repeat and are counted the number of times they occur.

Also, "00110011" is not a valid substring because all the 0's \(and 1's\) are not grouped together.

**Example 2:**

```text
Input: "10101"
Output: 4
Explanation: There are 4 substrings: "10", "01", "10", "01" that have equal 
   number of consecutive 1's and 0's.
```

**Note:**

* s.length will be between 1 and 50,000.
* s will only consist of "0" or "1" characters.

## Basic Idea:

从左往右扫描，维持变量 ones 和 zeros 来记录之前连续 1 和 0 的数量，如果当前连续 0 的数量小于等于之前连续 1 的数量，那么就可以形成一个符合条件的 substring，反之亦然。

* **Java Code:**

  ```java
    class Solution {
        public int countBinarySubstrings(String s) {
            int ones = 0, zeros = 0;
            int last = -1;
            int ret = 0;
            for (char c : s.toCharArray()) {
                if (c == '0') {
                    if (last == -1) {
                        zeros = 1;
                    } else if (last == 0) {
                        zeros++;
                    } else {
                        zeros = 1;
                    }
                    last = 0;
                    if (ones >= zeros) ret++;
                } else {
                    if (last == -1) {
                        ones = 1;
                    } else if (last == 1) {
                        ones++;
                    } else {
                        ones = 1;
                    }
                    last = 1;
                    if (zeros >= ones) ret++;
                }
            }
            return ret;
        }
    }
  ```

