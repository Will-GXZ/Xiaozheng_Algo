# Two Sum III - Data structure design

_update Jan 20,2018 23:20_

[LeetCode](https://leetcode.com/problems/two-sum-iii-data-structure-design/description/)

Design and implement a TwoSum class. It should support the following operations: add and find.

add - Add the number to an internal data structure. find - Find if there exists any pair of numbers which sum is equal to the value.

**For example,**

```text
add(1); add(3); add(5);
find(4) -> true
find(7) -> false
```

## Basic Idea:

使用一个 list 存储 all unique numbers，另外用一个 HashMap 存储每个数字出现的个数。在query的时候，遍历 list，找每个 value-number 是否存在。另外要注意，如果一个数字加上其本身等于 value，就需要判断该数字的数量是否超过 1。

## Java Code:

```java
    class TwoSum {
        private Map<Integer, Integer> map = new HashMap<>();
        /** Initialize your data structure here. */
        public TwoSum() {

        }

        /** Add the number to an internal data structure.. */
        public void add(int number) {
            map.put(number, map.getOrDefault(number, 0) + 1);
        }

        /** Find if there exists any pair of numbers which sum is equal to the value. */
        public boolean find(int value) {
            for (Map.Entry<Integer, Integer> entry : map.entrySet()) {
                int diff = value - entry.getKey();
                if (entry.getKey() == diff) {
                    if (entry.getValue() > 1) return true;
                } else if (map.containsKey(diff)) {
                    return true;
                }
            }
            return false;
        }
    }
```

_update May 6,2018 1:39_

## C++ Code

```cpp
    class TwoSum {
        unordered_map<int, int> _map;
    public:
        /** Initialize your data structure here. */
        TwoSum() {

        }

        /** Add the number to an internal data structure.. */
        void add(int number) {
            _map[number]++;
        }

        /** Find if there exists any pair of numbers which sum is equal to the value. */
        bool find(int value) {
            for (auto pair : _map) {
                int diff = value - pair.first;
                if (diff == pair.first) {
                    if (pair.second > 1) return true;
                } else if (_map.count(diff)) {
                    return true;
                }
            }
            return false;
        }
    };
```

