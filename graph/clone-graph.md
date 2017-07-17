## Clone Graph 
_update Jul 17, 2017 23:09_

---
[lintcode](http://www.lintcode.com/en/problem/clone-graph/#)

Clone an undirected graph. Each node in the graph contains a label and a list of its neighbors.

How we serialize an undirected graph:

Nodes are labeled uniquely.
     
     We use # as a separator for each node, and , as a separator for node label and each neighbor of the node.
     
     As an example, consider the serialized graph {0,1,2#1,2#2,2}.
     
     The graph has a total of three nodes, and therefore contains three parts as separated by #.
     
     First node is labeled as 0. Connect node 0 to both nodes 1 and 2.
     Second node is labeled as 1. Connect node 1 to node 2.
     Third node is labeled as 2. Connect node 2 to node 2 (itself), thus forming a self-cycle.
     Visually, the graph looks like the following:

        1
       / \
      /   \
     0 --- 2
          / \
          \_/
Have you met this question in a real interview? Yes
**Example**
return a deep copied graph.

#### Basic Idea:
题目所提供的只有一个node，显然要copy整个graph，我们需要先获得所有的node。比较正常的思路是先通过一个BFS获得所有的node，然后进行copy。

具体到如何进行copy，我们可以先尽力一个`hashmap<oldNode, newNode>`，然后遍历每个oldNode的neighbors，把每个neighbors中的oldNode对应的newNode加入对应newNode的neighbors中。

详见Code。

#### Python Code:
```python
    # Definition for a undirected graph node
    # class UndirectedGraphNode:
    #     def __init__(self, x):
    #         self.label = x
    #         self.neighbors = []
    class Solution:
        # @param node, a undirected graph node
        # @return a undirected graph node
        def __init__(self):
            self.dict = {}
            
        def cloneGraph(self, node):
            if not node: return None
            # 先用BFS找到所有node
            queue = collections.deque()
            visited = set()
            queue.appendleft(node)
            visited.add(node)
            while queue:
                nd = queue.pop()
                for neighbor in nd.neighbors:
                    if neighbor in visited: continue
                    queue.appendleft(neighbor)
                    visited.add(neighbor)
            
            # 现在visited已经存放了所有node，把他们放入dict中，新建新node与其一一对应
            for nd in visited:
                self.dict[nd] = UndirectedGraphNode(nd.label)
            
            # 把每个新dict的neighbor list用新的node一一对应
            for oldNode in self.dict:
                for oldNeighbor in oldNode.neighbors:
                    self.dict[oldNode].neighbors.append(self.dict[oldNeighbor])
            return self.dict[node]
```

#### Java Code:
```java
    /**
     * Definition for undirected graph.
     * class UndirectedGraphNode {
     *     int label;
     *     ArrayList<UndirectedGraphNode> neighbors;
     *     UndirectedGraphNode(int x) { label = x; neighbors = new ArrayList<UndirectedGraphNode>(); }
     * };
     */
    public class Solution {
        /**
         * @param node: A undirected graph node
         * @return: A undirected graph node
         */
        public UndirectedGraphNode cloneGraph(UndirectedGraphNode node) {
            if (node == null) return null;
            // 先用bfs找到所有node, 同时用一个map作为visited，同时为每个node匹配新的node
            Deque<UndirectedGraphNode> queue = new LinkedList<>();
            Map<UndirectedGraphNode, UndirectedGraphNode> map = new HashMap<>();
            queue.addFirst(node);
            map.put(node, new UndirectedGraphNode(node.label));
            while (! queue.isEmpty()) {
                UndirectedGraphNode nd = queue.removeLast();
                for (UndirectedGraphNode neighbor : nd.neighbors) {
                    if (map.containsKey(neighbor)) continue;
                    map.put(neighbor, new UndirectedGraphNode(neighbor.label));
                    queue.addFirst(neighbor);
                }
            }
            
            // 为每个新node匹配对应的adj list
            for (UndirectedGraphNode oldNode : map.keySet()) {
                UndirectedGraphNode newNode = map.get(oldNode);
                for (UndirectedGraphNode oldNeighbor : oldNode.neighbors) {
                    UndirectedGraphNode newNeighbor = map.get(oldNeighbor);
                    newNode.neighbors.add(newNeighbor);
                }
            }
            return map.get(node);
        }
    }
```