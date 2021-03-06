# LRU Cache

_update Aug 22, 2017 22:30_

[LintCode](http://www.lintcode.com/en/problem/lru-cache/#) [LeetCode](https://leetcode.com/problems/lru-cache/description/)

Design and implement a data structure for Least Recently Used \(LRU\) cache. It should support the following operations: get and put.

* get\(key\) - Get the value \(will always be positive\) of the key if the key exists in the cache, otherwise return -1.
* put\(key, value\) - Set or insert the value if the key is not already present. When the cache reached its capacity, it should invalidate the least recently used item before inserting a new item.

**Follow up:** Could you do both operations in O\(1\) time complexity?

**Example:**

```c
        LRUCache cache = new LRUCache( 2 /* capacity */ );

        cache.put(1, 1);
        cache.put(2, 2);
        cache.get(1);       // returns 1
        cache.put(3, 3);    // evicts key 2
        cache.get(2);       // returns -1 (not found)
        cache.put(4, 4);    // evicts key 1
        cache.get(1);       // returns -1 (not found)
        cache.get(3);       // returns 3
        cache.get(4);       // returns 4
```

### Basic Idea:

要实现一个O\(1\)时间完成get和set操作的 LRU cache 数据结构。以下分析摘自[这里](https://aaronice.gitbooks.io/lintcode/content/data_structure/lru_cache.html)。

* LRU，也就是least recently used，最近使用最少的；这样一个数据结构，能够保持一定的顺序，使得最近使用过的时间或者顺序被记录，实际上，具体每一个item最近一次何时被使用的，并不重要，重要的是在这样的一个结构中，item的相对位置代表了最近使用的顺序；满足这样考虑的结构可以是链表list或者数组array，不过前者更有利于insert和delete的操纵，此外，需要记录这个链表的head和tail，方便进行移动到tail或者删除head的操作，即：head.next作为最近最少使用的item，tail.prev为最近使用过的item，在set时，如果超出capacity，则删除head.next，同时将要插入的item放入tail.prev, 而get时，如果存在，只需把item更新到tail.prev即可。
* 这样set与get均为O\(1\)时间的操作 （HashMap Get/Set + LinkedList Insert/Delete\)，空间复杂度为O\(n\), n为capacity。

实现过程中要注意使用模块化的思路，写removeNode和insertAtTail两个helper function使代码看上去更易读。

### Python Code：

这段代码曾在LintCode的环境下ac；

```python
    class LinkedNode:
        def __init__(self, key=-1, val=-1):
            self.val = val
            self.key = key
            self.prev = None
            self.next = None

    class LRUCache:

        # @param capacity, an integer
        def __init__(self, capacity):
            self.CAPACITY = capacity
            self.head = LinkedNode()
            self.tail = LinkedNode()
            self.head.next = self.tail
            self.tail.prev = self.head
            self.table = {}


        # @return an integer
        def get(self, key):
            if key not in self.table:
                return -1
            else:
                node = self.table[key]
                self.removeNode(node)
                self.insertAtTail(node)
                return node.val

        # @param key, an integer
        # @param value, an integer
        # @return nothing
        def set(self, key, value):
            if key not in self.table:
                if len(self.table) == self.CAPACITY:
                    del self.table[self.head.next.key]
                    self.removeNode(self.head.next)
                node = LinkedNode(key, value)
                self.insertAtTail(node)
                self.table[key] = node
            else:
                node = self.table[key]
                node.val = value
                self.removeNode(node)
                self.insertAtTail(node)


        def insertAtTail(self, node):
            node.prev = self.tail.prev
            node.next = self.tail
            node.prev.next = node
            self.tail.prev = node

        def removeNode(self, node):
            node.prev.next = node.next
            node.next.prev = node.prev
```

### Java Code:

This code got AC in LeetCode;

```java
    class LRUCache {
        private class LinkedNode {
            int key;
            int val;
            LinkedNode prev = null;
            LinkedNode next = null;
            public LinkedNode() {
                key = -1;
                val = -1;
            }
            public LinkedNode(int key, int val) {
                this.key = key;
                this.val = val;
            }
        }

        private LinkedNode head;
        private LinkedNode tail;
        private Map<Integer, LinkedNode> map = null;
        private int CAPACITY;
        public LRUCache(int capacity) {
            this.head = new LinkedNode();
            this.tail = new LinkedNode();
            head.next = tail;
            tail.prev = head;
            this.map = new HashMap<>();
            this.CAPACITY = capacity;
        }

        public int get(int key) {
            if (! map.containsKey(key)) return -1;
            LinkedNode node = map.get(key);
            removeNode(node);
            insertAtTail(node);
            return node.val;
        }

        public void put(int key, int value) {
            if (! map.containsKey(key)) {
                if (map.size() == CAPACITY) {
                    // remove least recent used element
                    LinkedNode node = head.next;
                    map.remove(node.key);
                    removeNode(node);
                }
                // 建一个新的node，放入tail前
                LinkedNode node = new LinkedNode(key, value);
                map.put(key, node);
                insertAtTail(node);
            } else {
                // 原本已经有key，则找到node，改val，移到tail前
                LinkedNode node = map.get(key);
                node.val = value;
                removeNode(node);
                insertAtTail(node);
            }
        }

        private void removeNode(LinkedNode node) {
            node.prev.next = node.next;
            node.next.prev = node.prev;
        }

        private void insertAtTail(LinkedNode node) {
            node.next = tail;
            node.prev = tail.prev;
            node.prev.next = node;
            node.next.prev = node;
        }
    }
```

_update 2018-10-11 18:24:22_

### Update: Java 最终版本

```java
class LRUCache {
    private class Node {
        Node next, prev;
        int val, key;
        public Node(int key, int val) {
            this.val = val;
            this.key = key;
        }
    }

    private Node head, tail;
    private Map<Integer, Node> map;
    private int CAPACITY;

    public LRUCache(int capacity) {
        head = new Node(-1, -1);
        tail = new Node(-1, -1);
        head.next = tail;
        tail.prev = head;
        map = new HashMap<>();
        CAPACITY = capacity;
    }

    public int get(int key) {
        if (! map.containsKey(key)) return -1;
        Node node = map.get(key);
        moveToTail(node);
        return node.val;
    }

    public void put(int key, int value) {
        Node node = map.get(key);
        if (node == null) {
            if (map.size() == CAPACITY) {
                map.remove(head.next.key);
                remove(head.next);
            }
            node = new Node(key, value);
            addToTail(node);    
            map.put(key, node);
        } else {
            moveToTail(node);
            node.val = value;
        }
    }

    private void remove(Node node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }

    private void addToTail(Node node) {
        node.prev = tail.prev;
        node.next = tail;
        tail.prev.next = node;
        tail.prev = node;
    }

    private void moveToTail(Node node) {
        remove(node);
        addToTail(node);
    }
}
```

_update Feb 21 2019, 22:45_

## Java 真最终版本

这次将维持size的函数分离出来，叫做ensureCapacity\(\)，似的主要逻辑构架非常清晰明了。

```java
    class LRUCache {
        static class Node {
            Node prev = null, next = null;
            int key, val;
            public Node(int key, int val) {
                this.key = key;
                this.val = val;
            }
        }
        private Map<Integer, Node> map = new HashMap<>();
        private Node head, tail;
        private int CAPACITY;

        private void removeNode(Node node) {
            node.next.prev = node.prev;
            node.prev.next = node.next;
        }

        private void addToHead(Node node) {
            node.next = head.next;
            node.prev = head;
            node.next.prev = node;
            node.prev.next = node;
        }

        private void ensureCapacity() {
            if (map.size() <= CAPACITY) return;
            Node rmv = tail.prev;
            int key = rmv.key;
            map.remove(key);
            removeNode(rmv);
        }

        public LRUCache(int capacity) {
            head = new Node(0, 0);
            tail = new Node(0, 0);
            head.next = tail;
            tail.prev = head;
            CAPACITY = capacity;
        }

        public int get(int key) {
            if (!map.containsKey(key)) return -1;
            Node node = map.get(key);
            removeNode(node);
            addToHead(node);
            return node.val;
        }

        public void put(int key, int value) {
            if (map.containsKey(key)) {
                Node node = map.get(key);
                node.val = value;
                removeNode(node);
                addToHead(node);
            } else {
                Node node = new Node(key, value);
                map.put(key, node);
                addToHead(node);
                ensureCapacity();
            }
        }
    }
```

_update Aug 24, 2019_

### C++ 版本

注意在c++中实现链表我们需要内存管理，这里因为所有node都在map中，我们使用`unordered_map<int, unique_ptr<ListNode>>` 来管理heap上node的内存。

```cpp
class LRUCache {
    struct ListNode {
        int key;
        int val;
        ListNode* prev;
        ListNode* next;
    };
    unordered_map<int, unique_ptr<ListNode>> _map;
    ListNode head, tail;
    const int CAPACITY;

    void removeNode(ListNode* node) {
        node->prev->next = node->next;
        node->next->prev = node->prev;
    }

    void addLast(ListNode* node) {
        node->next = &tail;
        node->prev = tail.prev;
        tail.prev->next = node;
        tail.prev = node;
    }

    void moveToLast(ListNode* node) {
        removeNode(node);
        addLast(node);
    }

    void ensureCapacity() {
        if (_map.size() < CAPACITY) return;
        int key = head.next->key;
        removeNode(head.next);
        _map.erase(key);
    }
public:
    LRUCache(int capacity): CAPACITY(capacity) {
        head.next = &tail;
        tail.prev = &head;
    }

    int get(int key) {
        auto it = _map.find(key);
        if (it == _map.end()) return -1;
        ListNode* node = it->second.get();
        moveToLast(node);
        return node->val;
    }

    void put(int key, int value) {
        auto it = _map.find(key);
        if (it == _map.end()) {
            ensureCapacity();
            ListNode* node = new ListNode{key, value};
            addLast(node);
            _map.emplace(key, node);
        } else {
            ListNode* node = it->second.get();
            node->val = value;
            moveToLast(node);
        }
    }
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache* obj = new LRUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */
```

