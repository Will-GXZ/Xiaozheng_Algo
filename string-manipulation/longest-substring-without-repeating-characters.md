# Longest Substring Without Repeating Characters
_update Feb 17, 2018 15:03_

---
[LeetCode](https://leetcode.com/problems/longest-substring-without-repeating-characters/description/)

Given a string, find the length of the longest substring without repeating characters.

**Examples:**

```c
Given "abcabcbb", the answer is "abc", which the length is 3.

Given "bbbbb", the answer is "b", with the length of 1.

Given "pwwkew", the answer is "wke", with the length of 3. 

Note that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

<br>

## Basic Idea:
使用 sliding window 的思路，维持在 left 和 right 之间部分没有重复字符。具体地，用一个 `boolean dup` 来标记是否有重复，如果有，则不停右移 left 直到去掉重复。右移 right 来扩展window，每次检查是否产生了重复，如果是，则设 dup 为 true。具体地，检查是否有重复的方法是 keep 一个 map 来统计当前window中各个字符的个数。

* ### Java Code:
```java
