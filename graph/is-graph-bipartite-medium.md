# Is Graph Bipartite? (Medium)
_update Mar 4, 2018  13:33_

---
[LeetCode](https://leetcode.com/problems/is-graph-bipartite/description/)

Given a graph, return true if and only if it is bipartite.

Recall that a graph is bipartite if we can split it's set of nodes into two independent subsets A and B such that every edge in the graph has one node in A and another node in B.

The graph is given in the following form: `graph[i]` is a list of indexes `j` for which the edge between nodes `i` and `j` exists.  Each node is an integer between `0` and `graph.length - 1`.  There are no self edges or parallel edges: `graph[i]` does not contain `i`, and it doesn't contain any element twice.

**Example 1:**

    Input: [[1,3], [0,2], [1,3], [0,2]]
    Output: true
    Explanation: 
    The graph looks like this:
    0----1
    |    |
    |    |
    3----2
    We can divide the vertices into two groups: {0, 2} and {1, 3}.

**Example 2:**

    Input: [[1,2,3], [0,2], [0,1,3], [0,2]]
    Output: false
    Explanation: 
    The graph looks like this:
    0----1
    | \  |
    |  \ |
    3----2
    We cannot find a way to divide the set of nodes into two independent subsets.
 

**Note:**

1. `graph` will have length in range `[1, 100]`.
2. `graph[i]` will contain integers in range `[0, graph.length - 1]`.
3. `graph[i]` will not contain `i` or duplicate values.

<br>

## Basic Idea:
* ### 思路 1：（BFS + 2 sets）
用两个set `U and V` 表示两个 subset，确保一个set中的node的neighbor都在另一个set中，当发现有两个相连的node在同一个set中时，返回false；
  * #### Java Code:
```java
    class Solution {
        public boolean isBipartite(int[][] graph) {
            if (graph == null || graph.length == 0) return true;
            Set<Integer> U = new HashSet<>();
            Set<Integer> V = new HashSet<>();
            Deque<Integer> queue = new ArrayDeque<>();
            for (int node = 0; node < graph.length; ++node) {
                if (! U.contains(node) && ! V.contains(node)) {
                    queue.offerFirst(node);
                    while (! queue.isEmpty()) {
                        int curr = queue.pollLast();
                        for (int neighbor : graph[curr]) {
                            if (U.contains(curr) && U.contains(neighbor)
                               || V.contains(curr) && V.contains(neighbor)) {
                                return false;
                            } else if (U.contains(curr)) {
                                if (V.add(neighbor)) {
                                    queue.offerFirst(neighbor);
                                }
                            } else {
                                if (U.add(neighbor)) {
                                    queue.offerFirst(neighbor);
                                }
                            }
                        }
                    }
                }
            }
            return true;
        }
    }
```

* ### 思路 2：（DFS + 2 sets）
和之前的BFS思路类似，也是用两个set表示不同部分，然后再dfs的过程中将相连node放入不同的set，同时检查当前node和其neighbor是否在同一个set中；时间复杂度都是 `O(|E|)`，因为都遍历了所有的边；
  * #### Java Code:
```java
    class Solution {
        public boolean isBipartite(int[][] graph) {
            if (graph == null || graph.length == 0) return true;
            Set<Integer> U = new HashSet<>();
            Set<Integer> V = new HashSet<>();
            for (int node = 0; node < graph.length; ++node) {
                if (U.contains(node) || V.contains(node)) {
                    continue;
                } else {
                    U.add(node);
                    if (! dfs(graph, node, U, V)) return false;
                }
            }
            return true;
        }
    
        private boolean dfs(int[][] graph, int curr, Set<Integer> U, Set<Integer> V) {
            for (int neighbor : graph[curr]) {
                if (U.contains(curr) && U.contains(neighbor) 
                   || V.contains(curr) && V.contains(neighbor)) {
                    return false;
                } else if (U.contains(curr)) {
                    if (V.add(neighbor)) {
                        if (! dfs(graph, neighbor, U, V)) return false;
                    }
                } else {
                    if (U.add(neighbor)) {
                        if (! dfs(graph, neighbor, U, V)) return false;
                    }
                }
            }
            return true;
        }
    }
```

* ### 思路 3： （Union Find 并查集）
既然是将图分成两个部分，并且需要随时检查两个node是否属于同一部分，我们可以使用并查集求解。只是这道题用并查集并没有太大的优势，直接用set可能会更加简单，所以并查集的解法就当做一个练习吧；
  * #### Java Code:
```java





























