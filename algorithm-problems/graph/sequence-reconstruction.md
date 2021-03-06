# Sequence Reconstruction

_update Jul 18, 2017 15:35_

[lintcode](http://www.lintcode.com/en/problem/sequence-reconstruction/)

Check whether the original sequence org can be uniquely reconstructed from the sequences in seqs. The org sequence is a permutation of the integers from 1 to n, with 1 ≤ n ≤ 10^4. Reconstruction means building a shortest common supersequence of the sequences in seqs \(i.e., a shortest sequence so that all sequences in seqs are subsequences of it\). Determine whether there is only one sequence that can be reconstructed from seqs and it is the org sequence.

**Example**

```text
Given org = [1,2,3], seqs = [[1,2],[1,3]]
Return false
Explanation:
[1,2,3] is not the only one sequence that can be reconstructed, 
because [1,3,2] is also a valid sequence that can be reconstructed.

Given org = [1,2,3], seqs = [[1,2]]
Return false
Explanation:
The reconstructed sequence can only be [1,2].

Given org = [1,2,3], seqs = [[1,2],[1,3],[2,3]]
Return true
Explanation:
The sequences [1,2], [1,3], and [2,3] can uniquely reconstruct 
the original sequence [1,2,3].

Given org = [4,1,5,2,6,3], seqs = [[5,2,6,3],[4,1,5,2]]
Return true
```

## Basic Idea:

这道题其实就是求是否有唯一的topological sort的结果。什么情况下会有不止一种结果呢？就是当某一时刻有不止一个indegree为0的node的时候。所以我们只需要监测`queue.size() > 1`即可。当然，在这之前要根据 seqs 生成 `Map<Integer, List<Integer>>` 形式的 graph；

具体的，有一些非法输入的情况需要handle。

## Java Code:

```java
public class Solution {
    /**
     * @param org: a permutation of the integers from 1 to n
     * @param seqs: a list of sequences
     * @return: true if it can be reconstructed only one or false
     */
    public boolean sequenceReconstruction(int[] org, int[][] seqs) {
        Map<Integer, List<Integer>> graph = initGraph(seqs);
        Map<Integer, Integer> indegreeCount = new HashMap<>();
        for (Map.Entry<Integer, List<Integer>> entry : graph.entrySet()) {
            if (! indegreeCount.containsKey(entry.getKey())) {
                indegreeCount.put(entry.getKey(), 0);
            }
            for (int neighbor : entry.getValue()) {
                indegreeCount.put(neighbor, indegreeCount.getOrDefault(neighbor, 0) + 1);
            }
        }

        // 每次将当前node的出边所连接的neighbors的indegree减一，若之后indegree为0，则入队。
        // 如果有一刻queue的size大于1，说明有不唯一的topological order，之间返回 false
        Deque<Integer> queue = new ArrayDeque<>();
        for (Map.Entry<Integer, Integer> entry : indegreeCount.entrySet()) {
            if (entry.getValue() == 0) queue.offerLast(entry.getKey());
        }

        List<Integer> sortedList = new ArrayList<>();
        while (! queue.isEmpty()) {
            if (queue.size() > 1) return false;
            int node = queue.pollFirst();
            sortedList.add(node);
            for (int neighbor : graph.get(node)) {
                indegreeCount.put(neighbor, indegreeCount.get(neighbor) - 1);
                if (indegreeCount.get(neighbor) == 0) queue.offerLast(neighbor);
            }
        }

        // 检查是否有环
        for (Map.Entry<Integer, Integer> entry : indegreeCount.entrySet()) {
            if (entry.getValue() != 0) return false;
        }

        // 检查 topological sort 之后的 list 是否和 org 相同
        if (org.length != sortedList.size()) return false;
        for (int i = 0; i < org.length; ++i) {
            if (sortedList.get(i) != org[i]) return false;
        }
        return true;
    }

    private Map<Integer, List<Integer>> initGraph(int[][] seqs) {
        Map<Integer, List<Integer>> graph = new HashMap<>();
        for (int[] seq : seqs) {
            if (seq.length == 1) {
                if (! graph.containsKey(seq[0])) {
                    graph.put(seq[0], new ArrayList<>());
                }
            }
            for (int i = 1; i < seq.length; ++i) {
                int a = seq[i - 1], b = seq[i];
                if (! graph.containsKey(a)) {
                    graph.put(a, new ArrayList<Integer>());
                }
                if (! graph.containsKey(b)) {
                    graph.put(b, new ArrayList<Integer>());
                }
                graph.get(a).add(b);
            }
        }
        return graph;
    }
}
```

## Python Code:

```python
    class Solution:
        # @param {int[]} org a permutation of the integers from 1 to n
        # @param {int[][]} seqs a list of sequences
        # @return {boolean} true if it can be reconstructed only one or false
        def sequenceReconstruction(self, org, seqs):
            def initGraph(nodes, seqs):
                count = 0
                graph = {}
                for node in nodes:
                    graph[node] = set()
                for edges in seqs:
                    count += len(edges)
                    if len(edges) == 1:
                        if edges[0] < 1 or edges[0] > len(nodes):
                            return None # case: [[100000000]]
                    for i in range(1, len(edges)):
                        u, v = edges[i - 1], edges[i]
                        if (min(u, v) < 1 or max(u, v) > len(nodes)):
                            return None # case: [[-1, 1000000000]]
                        graph[u].add(v)
                if count < len(nodes): 
                    return None # case: [1], [[],[]]
                return graph


            graph = initGraph(org, seqs)
            if graph is None: 
                return False
            # count indegree
            count = {}
            for node in graph:
                count[node] = 0
            for node in graph:
                for neighbor in graph[node]:
                    count[neighbor] += 1

            # BFS, topological sort, 看queue的size是否会大于1
            queue = collections.deque()
            for node in graph:
                if count[node] == 0:
                    queue.appendleft(node)
            while queue:
                if len(queue) > 1:
                    return False
                node = queue.pop()
                for neighbor in graph[node]:
                    count[neighbor] -= 1
                    if count[neighbor] == 0:
                        queue.appendleft(neighbor)

            return not [node for node in graph if count[node] > 0]
```

_update Aug 29, 2019_

再记录一个更加快速的解法，不用contruct graph，而是从边出发。我们先从给定的所有seq中获得所有solid的edge，检查每个edge是否vialate给定的org array顺序。又因为必须是唯一存在的顺序，之后也要检查从前到后每一对org中元素相对顺序是否有对应的edge在seqs中出现。

例如：

```c
    对于 org=[2,3,5,4,1]
    则在seqs所给定的所有关系中，必须存在:
        edge: [2,3] [3,5] [5,4] [4,1]
```

另外还有一些特殊情况要处理。

```cpp
class Solution {
public:
    bool sequenceReconstruction(vector<int>& org, vector<vector<int>>& seqs) {
        // 记录 number->index mapping
        vector<int> pos(org.size() + 1);
        for (int i{0}; i < org.size(); ++i) {
            pos[org[i]] = i;
        }
        unordered_map<int, unordered_set<int>> edges;
        bool hasEdge = false; // 如果seq全部为空，返回false
        for (auto& seq : seqs) {
            for (int i{0}; i < seq.size(); ++i) {
                hasEdge = true;
                if (seq[i] < 1 || seq[i] > org.size()) {
                    return false;
                }
                // 查看是否冲突，记录保存edge
                if (i > 0) {
                    int u{seq[i - 1]}, v{seq[i]};
                    if (pos[u] >= pos[v]) return false;
                    edges[u].emplace(v);
                }
            }
        }
        if (!hasEdge) return false;
        // 检查是否每对顺序都有对应edge
        for (int i{1}; i < org.size(); ++i) {
            if (!edges[org[i - 1]].count(org[i])) return false;
        }
        return true;
    }
};
```

